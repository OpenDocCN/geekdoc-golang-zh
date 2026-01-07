# 高流量 Go 服务中 net/http、net.Conn 和 UDP 的有效使用[]

> 原文：[`goperf.dev/02-networking/efficient-net-use/`](https://goperf.dev/02-networking/efficient-net-use/)

当我们最初在 Go 中构建高流量服务时，我们通常会大量依赖 `net/http`。它稳定、易用，对于 80% 的用例来说非常出色。但是，一旦流量激增或延迟预算减少，问题就开始显现。

并非 `net/http` 出了问题——只是默认设置是为了方便而调整的，而不是在压力下的性能。随着我们将后端服务扩展到每秒处理数百万个请求，了解抽象之下的发生情况成为满足 SLO 和生产中的救火之间的区别。

本文将指导如何使网络化的 Go 服务真正高效——什么有效，什么无效，以及如何超越惯用用法。我们将从 `net/http` 开始，深入到原始的 `net.Conn`，并以处理延迟敏感系统中 UDP 的实际模式结束。

## 简单 HTTP 调用背后的隐藏复杂性

让我们从大多数 Go 开发者开始的地方开始：一个简单的 `http.Client`。

```go
client := &http.Client{
    Timeout: 5 * time.Second,
}

resp, err := client.Get("http://localhost:8080/data")
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close() 
```

这看起来无害。它完成了工作，并且在大多数本地测试中表现合理。但在生产环境中，在规模上，这种看似无辜的代码可能会触发一系列令人惊讶的问题：泄漏的连接、内存激增、阻塞的 goroutine 和神秘的延迟悬崖。

最常见的问题之一是关闭前忘记完全读取 `resp.Body`。[Go 的 HTTP 客户端只有在内容被完全读取后才会重用连接](https://github.com/google/go-github/pull/317)。在负载下，这意味着您会不断打开新的 TCP 连接——对内核的临时端口进行冲击，耗尽文件描述符，并触发节流。

这里有一个安全的模式：

```go
io.Copy(io.Discard, resp.Body)
defer resp.Body.Close() 
```

## 调整传输：当默认设置不足时

很容易忽视 `http.DefaultTransport` 后面隐藏了多少全局状态。如果您在应用程序中启动多个未自定义传输的 `http.Client` 实例，您可能在不意识到的情况下正在重用共享的全局池。

这会导致在负载下的不可预测行为：空闲连接被过快地移除，或者保持连接的时间比应有的要长。解决方案？构建一个与您的并发配置相匹配的调整过的 `Transport`。

### 可调整的自定义 `http.Transport` 字段

以下所有设置都是 `http.Transport` 结构体的一部分：

```go
transport := &http.Transport{
    MaxIdleConns:          1000,
    MaxConnsPerHost:       100,
    IdleConnTimeout:       90 * time.Second,
    ExpectContinueTimeout: 0,
    DialContext: (&net.Dialer{
        Timeout:   5 * time.Second,
        KeepAlive: 30 * time.Second,
    }).DialContext,
}

client := &http.Client{
    Transport: transport,
    Timeout:   2 * time.Second,
} 
```

## 更高级的优化技巧

这些都与 `http.Transport`、`http.Client` 和 `http.Server` 结构体中的关键设置相关联，或者是在它们之上构建的自定义包装器：

### 谨慎设置 `ExpectContinueTimeout`

如果我们的客户端发送大的 POST 请求，而服务器没有正确支持 `100-continue`，我们可以减少或消除这种延迟：

```go
transport := &http.Transport{
    ...
    ExpectContinueTimeout: 0, // if not needed, skip the wait entirely
    ...
} 
```

### 限制 `MaxConnsPerHost`

Go 的默认 HTTP 客户端会为一个主机打开无限数量的连接。这很好，直到您的下游之一无法处理。

```go
transport := &http.Transport{
    ...
    MaxConnsPerHost: 100,
    ...
} 
```

这防止了高峰期间的冲击，并避免了耗尽后端服务的资源。

### 使用小的 `http.Client.Timeout`

一个常见的错误是设置非常高的超时时间（例如，30 秒）以确保安全。但是，长时间的超时会在压力下保持 goroutines、缓冲区和套接字。更喜欢更紧密的控制：

```go
client := &http.Client{
    Timeout: 2 * time.Second,
} 
```

而不是依赖于大的超时，使用带有退避的重试（例如，使用 go-retryablehttp）来提高部分失败情况下的弹性。

### 在 `http.Server` 中显式设置 `ReadBufferSize` 和 `WriteBufferSize`

Go 的 `http.Server` 没有直接暴露 `ReadBufferSize` 和 `WriteBufferSize`，但在您需要减少 GC 压力和提高负载下的系统调用效率时，您可以在自定义的 `Conn` 包装器中预先设置缓冲区的大小。对于大多数工作负载来说，4KB–8KB 是一个平衡的值：它足够大，可以有效地处理小的头和体，而不会浪费内存。例如，4KB 可以覆盖几乎所有典型的 HTTP 头和小的 JSON 有效负载。

您可以通过在包装连接中使用 `bufio.NewReaderSize` 和 `NewWriterSize` 来实现这一点，该连接连接到自定义的 `net.Listener` 和 `http.Server.ConnContext`。

如果您使用 `fasthttp`，您可以显式配置缓冲区大小：

```go
server := &fasthttp.Server{
    ReadBufferSize:  4096,
    WriteBufferSize: 4096,
    ...
} 
```

这避免了每次请求时的动态分配，并在高吞吐量下导致更可预测的内存使用和缓存局部性。

### 使用 `bufio.Reader.Peek()` 进行高效的帧化

当在 TCP 上实现帧协议时，如长度前缀的二进制消息，在循环中天真地调用 `Read()` 可能会导致读取碎片化和不必要的系统调用。这会累积起来，尤其是在负载下。使用 `Peek()` 允许您查看缓冲区数据而不前进读取位置，这使得在没有触发额外读取的情况下更容易检测消息边界。这在需要严格控制帧化的流式系统或复用连接中是一种实用的技术。

```go
header, _ := reader.Peek(8) // Peek without advancing the buffer 
```

### 使用自定义拨号器强制进行新的 DNS 查询

Go 的内置 DNS 缓存持续整个进程的生命周期。在动态环境中，如 Kubernetes，当服务 IP 发生变化但客户端继续重用过时的 IP 时，这可能会成为一个问题。为了避免这种情况，您可以通过为每个请求创建一个新的 `net.Dialer` 或定期轮换 HTTP 客户端来强制进行新的 DNS 查询。

但在需要时，您可以绕过 Go 的内部 DNS 缓存：

```go
DialContext: func(ctx context.Context, network, addr string) (net.Conn, error) {
    return (&net.Dialer{}).DialContext(ctx, network, addr)
}, 
```

这确保了每个请求都进行一次新的 DNS 查询。虽然这会增加一些开销，但在故障转移敏感的环境中这是必要的。

### 使用`sync.Pool`为读取器/写入器

大多数人使用`sync.Pool`来重用`[]byte`缓冲区，但对于每秒处理许多请求的服务，为每个连接分配`bufio.Reader`和`bufio.Writer`对象会累积起来。这些对象还维护自己的缓冲区，因此回收它们可以减轻堆分配和垃圾回收的压力。

```go
var readerPool = sync.Pool{
    New: func() interface{} {
        return bufio.NewReaderSize(nil, 4096)
    },
}

func getReader(conn net.Conn) *bufio.Reader {
    r := readerPool.Get().(*bufio.Reader)
    r.Reset(conn)
    return r
} 
```

这种做法显著减少了分配的波动并提高了延迟一致性，尤其是在处理数千个并发连接的系统。

### 不要在多个主机之间共享`http.Client`

虽然重用单个`http.Client`可能看起来效率更高，但每个目标主机在底层的`http.Transport`中维护自己的内部连接池。如果你为多个基本 URL 使用相同的客户端，你最终会混合连接重用，导致无关服务之间的头阻塞。更糟糕的是，DNS 缓存和套接字耗尽变得难以追踪。

相反，为每个你与之交互的上游服务创建一个专门的`http.Client`。这提高了连接的重用性，避免了服务之间的串扰，并且通常使行为更加可预测，尤其是在服务网格或处理多个外部 API 的环境中。

### 使用`ConnContext`和`ConnState`钩子进行调试

这些钩子对于跟踪每个连接的生命周期非常有用——尤其是在调试生产环境中的内存泄漏、连接停滞或资源耗尽等问题时。`ConnState`回调提供了对`StateNew`、`StateActive`、`StateIdle`和`StateHijacked`等转换的可见性，允许你根据连接状态进行日志记录、跟踪或应用自定义处理。

通过监控这些事件，你可以检测到连接挂起、无法关闭或意外空闲的情况。这也有助于将行为与客户端 IP 或网络区域相关联。

```go
ConnState: func(conn net.Conn, state http.ConnState) {
    log.Printf("conn %v changed state to %v", conn.RemoteAddr(), state)
}, 
```

## 放弃抽象：何时使用`net.Conn`

随着我们接近 Go 标准库所能提供的极限，了解存在专门为事件驱动、低延迟工作负载构建的高性能替代品是很有价值的。例如，`cloudwego/netpoll`（[GitHub 链接](https://github.com/cloudwego/netpoll)）和`tidwall/evio`（[GitHub 链接](https://github.com/tidwall/evio)）等项目提供了超越仅使用`net.Conn`所能实现性能的强大工具。

+   **[`cloudwego/netpoll`](https://github.com/cloudwego/netpoll)** 是一个基于 epoll 的网络库，旨在以最小的 GC 开销构建大规模并发网络服务。它使用基于事件的 I/O 来消除每个连接的 goroutine 成本，非常适合像 RPC 代理、内部服务网格或高频消息系统这样的场景。

+   **[`tidwall/evio`](https://github.com/tidwall/evio)** 提供了一个基于 [reactor 模式](https://en.wikipedia.org/wiki/Reactor_pattern) 的快速、非阻塞的事件循环，适用于对延迟敏感而连接状态复杂性较低的协议，例如自定义 TCP、UDP 协议或轻量级网关。

如果你正在构建吞吐量或连接数超过数十万，或尾延迟至关重要的系统，探索这些库是值得的。它们有权衡——最明显的是标准化程度较低和更多手动生命周期管理——但作为回报，它们让你能够对性能关键路径进行精细控制。

有时候，即使是调优过的 HTTP 栈也不够。

在处理内部二进制协议或每秒处理数十万请求的服务等情况下，我们可能会发现我们正在为不使用的 HTTP 语义付费。降低到 `net.Conn` 给我们提供了完全的控制权——没有池化惊喜，没有隐藏的保活，只是一个原始套接字。

```go
ln, err := net.Listen("tcp", ":9000")
... 
```

这使我们能够完全接管连接生命周期、缓冲和并发。它还开辟了通过缓冲区重用减少 GC 影响的机会：

```go
var bufPool = sync.Pool{
    New: func() interface{} {
        return make([]byte, 4096)
    },
} 
```

在延迟敏感的系统中启用 TCP_NODELAY 很有用：

```go
tcpConn := conn.(*net.TCPConn)
tcpConn.SetNoDelay(true) 
```

## 超越 TCP：为什么 UDP 很重要

对于像日志火流摄取、遥测信标或心跳消息这样的工作负载，TCP 可能太重了。我们可以转向 UDP 以实现低延迟、无连接的数据传输：

```go
conn, _ := net.ListenUDP("udp", &net.UDPAddr{Port: 9999})
... 
```

这跳过了握手并有效地重用了套接字。但请记住——UDP 不提供排序、可靠性或内置会话跟踪。它在高流量、低后果的管道中表现最佳。

## 选择合适的工具

我们的网络策略应该反映流量形状和协议预期：

| 场景 | 推荐工具 |
| --- | --- |
| REST/gRPC，通用 API | `net/http` |
| 高负载下的 HTTP | 调优的 `http.Transport` |
| 自定义 TCP 协议 | `net.Conn` |
| 帧化二进制数据 | `net.Conn` + 缓冲区管理 |
| 一触即发的遥测 | `UDPConn` |
| 延迟敏感的游戏更新 | `UDP` |

* * *

在规模上，网络性能永远不仅仅是关于网络。它还关乎内存压力、上下文生命周期、内核行为和套接字卫生。我们可以利用 Go 标准库走得很远，但当系统反推时，我们需要深入挖掘。

好消息？Go 给我们提供了工具。我们只需要明智地使用它们。

如果你正在尝试帧协议、零拷贝解析或自定义基准测试设置，还有很多东西可以探索。让我们继续前进。
