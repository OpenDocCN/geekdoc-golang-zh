# 调整 Go 服务中的 DNS 性能

> [原文链接](https://goperf.dev/02-networking/dns_performance/)

DNS 解析通常不会引起注意，但它仍然可能会减慢 Go 应用程序的速度。即使在查找过程中出现短暂的延迟，在组件频繁通信的分布式或微服务架构中，这些延迟也会累积。了解 Go 在底层如何解析 DNS 以及如何调整它，可以使您的服务更加响应迅速和可靠。

## Go 中 DNS 解析的工作原理：cgo 与原生解析器

Go 支持两种处理 DNS 查询的不同方式：内置的 **纯 Go** 解析器和基于 **cgo** 的解析器。

`纯 Go` 解析器完全自包含，避免使用任何外部 DNS 库。它从 `/etc/resolv.conf`（在类 Unix 系统上）读取其配置，并直接与配置的名称服务器通信。这使得它简单且通常性能良好，尽管它有时难以处理更复杂或高度定制的 DNS 环境。

相反，基于 **cgo** 的解析器将 DNS 解析委托给操作系统的自身解析器（通过 C 标准库 `libc`）。这提供了与复杂或定制 DNS 环境更好的兼容性——例如涉及 LDAP 或组播 DNS 的环境——但它也带来了一定的权衡。cgo 解析器由于调用外部 C 库而增加了开销，有时还可能导致线程安全或不可预测的延迟峰值。

可以通过环境变量显式配置 Go 以优先使用 **纯 Go** 解析器：

```go
`export  GODEBUG=netdns=go` 
```

或者，强制使用 cgo 解析器：

```go
`export  GODEBUG=netdns=cgo` 
```

### 运行时依赖

启用 cgo 改变了 Go 二进制文件与操作系统的交互方式。启用 cgo 后，二进制文件不再独立存在——它动态链接到 `libc` 和系统加载器，这在 `ldd` 的输出中可以揭示。

```go
`$  ldd  ./app-cgo   linux-vdso.so.1  (0x0000fa34ddbad000)   libc.so.6  =>  /lib/aarch64-linux-gnu/libc.so.6  (0x0000fa34dd9b0000)   /lib/ld-linux-aarch64.so.1  (0x0000fa34ddb70000)` 
```

启用 cgo 的二进制文件依赖于系统的 C 运行时（`libc.so.6`）和动态加载器（`ld-linux`）。如果没有在宿主机上提供这些共享库，二进制文件将无法启动——这使得它不适用于像 scratch 容器这样的简化环境。

相比之下，纯 Go 二进制文件完全自包含并静态链接。如果您在它上面运行 `ldd`，您将只看到：

```go
`$  ldd  ./app-pure   not  a  dynamic  executable` 
```

这表明二进制文件所需的全部代码已经内置，无需在运行时依赖共享库。正因为如此，纯 Go 构建非常适合最小化容器或没有 C 运行时的裸环境，提供了更好的可移植性和更少的操作意外。然而，这些二进制文件无法利用需要 cgo 和宿主机的 `libc` 的系统级解析器功能。

## DNS 缓存：为什么和何时使用

缓存 DNS 结果可以防止应用程序反复发送相同的查询，这可以消除可感知的延迟。每次查找至少涉及一次网络往返，当在规模上发生时，累积的成本可能会成为服务性能的严重拖累。

许多操作系统和托管环境已经包含某种形式的 DNS 缓存。在 Windows 上，`DNS 客户端`服务保留本地缓存；在 macOS 上，`mDNSResponder`处理这个问题；在大多数 Linux 系统上，`systemd-resolved`或`nscd`通常提供缓存层。在云环境中，DNS 查询通常通过同一数据中心附近的一个缓存解析器路由。这些机制确实有助于减少延迟，但它们对应用程序是透明的，并且对 TTLs 和策略的可见性和控制很少。在 Linux 上，如果`systemd-resolved`未启用，除非你明确配置，否则根本不会发生任何缓存。

由于 Go 本身不包含任何 DNS 缓存，如果你想实现细粒度、应用级别的控制，你必须自己实现。一个选项是运行你自己的缓存 DNS 服务器；另一个选项是将轻量级缓存直接嵌入到你的代码中，使用第三方库。

使用[go-cache](https://github.com/patrickmn/go-cache)的简单 DNS 缓存示例：

```go
`import  (   "github.com/patrickmn/go-cache"   "net"   "time" )  var  dnsCache  =  cache.New(5*time.Minute,  10*time.Minute)  func  LookupWithCache(host  string)  ([]net.IP,  error)  {   if  cachedIPs,  found  :=  dnsCache.Get(host);  found  {   return  cachedIPs.([]net.IP),  nil   }    ips,  err  :=  net.LookupIP(host)   if  err  !=  nil  {   return  nil,  err   }   dnsCache.Set(host,  ips,  cache.DefaultExpiration)   return  ips,  nil }` 
```

过度使用 DNS 缓存有其缺点——它可能会导致你服务过时的记录，如果上游地址更改，还会使你的服务变得脆弱。调整你的缓存过期时间，使其反映你依赖的域名实际更改的频率是值得的。

## 使用自定义拨号器和预解析 IP 地址

在对延迟敏感的服务中，提前解析 DNS 或使用自定义拨号器显式控制解析通常是有意义的。每次调用`net.Dial`或`net.DialContext`与主机名都会触发一个查找，这可能涉及系统调用、上下文切换，有时甚至如果缓存是冷的，还会涉及网络往返。在高吞吐量下，这种开销会累积。

为了消除这种开销，你可以在启动时解析主机名并保存结果 IP 地址。这种方法在处理一组固定且很少更改的端点时特别有效。

```go
`var  serviceAddr  string  func  init()  {   ips,  err  :=  net.LookupIP("api.example.com")   if  err  !=  nil  ||  len(ips)  ==  0  {   panic("Unable to resolve api.example.com")   }   serviceAddr  =  ips[0].String()  // in real code, consider picking an IP randomly, prefer IPv4 if needed, or iterate over ips with checks as appropriate }` 
```

一个缺点是它会固定进程生命周期的 IP 地址，所以如果端点更改了地址，连接可能会开始失败。为了优雅地处理这个问题，你可以运行一个后台 goroutine，定期刷新解析的 IP 地址。

自定义拨号器更进一步：它们允许你控制基于每个连接的 DNS 解析和套接字建立方式。这将使你能够强制通过特定的解析器建立连接，硬编码预解析的 IP 地址，或者甚至通过直接拨打电话地址来完全绕过 DNS。

```go
`import  (   "net"   "context"   "time" )  var  dialer  =  &net.Dialer{   Timeout:  5  *  time.Second,   KeepAlive:  30  *  time.Second,   Resolver:  &net.Resolver{   PreferGo:  true,   Dial:  func(ctx  context.Context,  network,  address  string)  (net.Conn,  error)  {   return  net.Dial(network,  "8.8.8.8:53")   },   }, }  func  ConnectWithCustomDialer(ctx  context.Context,  address  string)  (net.Conn,  error)  {   return  dialer.DialContext(ctx,  "tcp",  address) }` 
```

自定义拨号器也可以绕过不可靠的系统解析器或通过专用、更快的域名服务器直接进行 DNS 查询。它们让你能够精确控制解析过程，但这种控制也带来了额外的复杂性——你需要自己处理回退和刷新逻辑。

## 实际 DNS 缓慢的指标和调试技巧

识别和排除由 DNS 引起的延迟需要深入的指标和有针对性的调试技术。测量 DNS 不仅仅是看它有多快——它有助于回答更深层次的问题：查找是否击中了操作系统或提供商的缓存？通往 DNS 服务器的网络路径是否不可靠？是否有特定的域名服务器比其他服务器慢？IPv6 和 IPv4 的行为是否不同？没有可见性，DNS 问题可能会无声地降低性能和可靠性。

### 指标

确保跟踪你的服务指标中的 DNS 解析时间，这样你就可以看到查找实际上为每个请求增加了多少时间。最简单的方法是在查找之前和完成后记录持续时间的计时器包装你的 DNS 调用。随着时间的推移，这些测量有助于你识别趋势，发现间歇性缓慢，并将 DNS 延迟与系统其他部分相关联。

```go
`start  :=  time.Now() ips,  err  :=  net.LookupIP("example.com") duration  :=  time.Since(start)  recordDNSLookupDuration("example.com",  duration)` 
```

查看高百分位延迟——如第 95 或第 99 百分位——可以揭示平均可能隐藏的间歇性缓慢或不可靠的 DNS 行为。

### 调试技巧

面对无法解释的延迟峰值时，利用 Go 的内置调试模式：

```go
`export  GODEBUG=netdns=2` 
```

启用此功能将生成详细的 DNS 查询日志，显示每个请求从开始到结束的精确处理方式。你可以看到服务器响应缓慢的时间，查找失败并触发重试的时间，或者运行时意外回退到 cgo 解析器的时间。这种详细的洞察使得定位标准指标经常错过的难以捉摸的 DNS 问题变得更加容易。

### 高级 DNS 性能技巧

在你的服务附近运行本地 DNS 缓存解析器，如`dnsmasq`或`Unbound`，可以消除外部查找的额外延迟。如果安全和隐私是担忧，启用 DNS-over-HTTPS (DoH)或 DNS-over-TLS (DoT)也是一个选项，尽管它由于加密而带来了一些额外的延迟。最后，审查和调整你的`/etc/resolv.conf`——调整重试次数和超时设置——有助于确保解析器在负载或故障条件下表现出可预测的行为。
