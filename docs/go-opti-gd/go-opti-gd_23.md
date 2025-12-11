# 在 Go 中管理 10K+并发连接

> [`goperf.dev/02-networking/10k-connections/`](https://goperf.dev/02-networking/10k-connections/)

<details class="info"><summary>为什么不是 100K+或 1 百万连接？</summary>

虽然将挑战定位为“100K 并发连接”很有吸引力，但实际工程通常从一个更实际的靶标开始：10K 到 20K 稳定、高性能的连接。这并不是 Go 本身的限制，而是现实世界约束的反映：ulimit 设置、临时端口的可用性、TCP 堆栈配置以及应用程序工作负载的性质都设定了硬边界。

云环境引入了它们自己的考虑因素。例如，AWS Fargate 明确地将软限制和硬限制（打开文件数）都设置为 65,535，这为 socket 密集型应用提供了更多的空间，但仍然低于 100K+的阈值。在 EC2 实例上，实际限制取决于基础操作系统和用户配置。默认情况下，许多 Linux 发行版为 nofile 设置了 1024 的软限制和 65535 的硬限制。即使这个硬限制也低于单个进程中处理 10 万个打开连接所需的限制。达到更高的限制需要内核级别的调整、容器运行时覆盖和多进程策略来分配文件描述符负载。

处理简单回声逻辑的服务器与执行 CPU 密集型处理、结构化日志记录或实时转换的服务器行为非常不同。此外，平台级别的可调整性各不相同——Linux 通过 sysctl、epoll 和 reuseport 提供了细粒度控制，而 macOS 缺乏许多这些机制。在这种情况下，在真实工作负载中实现并维持 10K+并发连接是一个具有挑战性但实用的基准。</details>

在 Go 中处理大量并发通常被浪漫化——“goroutines 很便宜，只需创建它们！”——但随着我们向六位数并发级别推进，现实变得更加严峻。仅通过扩展硬件来服务超过 10,000 个并发套接字并不是解决问题的方法——它需要一个与操作系统、Go 运行时和网络堆栈协同工作的架构，而不是与之对抗。

## 接受 Go 的并发模型

Go 的轻量级 goroutines 和其强大的运行时调度器使其成为扩展网络应用的绝佳选择。Goroutines 仅消耗几 KB 的堆栈空间，从理论上讲，这使得它们非常适合处理数万个并发连接。然而，现实迫使我们必须超越仅仅启动 goroutines。虽然语言的抽象使并发几乎“神奇”，但要在这个规模上实现真正的效率需要有意的设计。

运行一个为每个连接创建一个 goroutine 的服务器意味着你过度依赖运行时调度器来处理数千个并发执行路径。虽然 goroutine 很轻量级，但它们不是免费的——每个 goroutine 都会增加内存消耗，并引入与并发性成比例的调度开销。因此，应该首先采用的设计模式是确保每个连接遵循一个明确定义的生命周期，并且每个 goroutine 都能尽可能高效地完成任务。

让我们考虑一个基本模型，其中我们接受连接并将它们的处理委托给单独的 goroutine：

```go
`package  main  import  (   "log"   "net"   "sync/atomic"   "time" )  var  activeConnections  uint64  func  main()  {   listener,  err  :=  net.Listen("tcp",  ":8080")   if  err  !=  nil  {   log.Fatalf("Error starting TCP listener: %v",  err)   }   defer  listener.Close()    for  {   conn,  err  :=  listener.Accept()   if  err  !=  nil  {   log.Printf("Error accepting connection: %v",  err)   continue   }    atomic.AddUint64(&activeConnections,  1)   go  handleConnection(conn)   } }  func  handleConnection(conn  net.Conn)  {   defer  func()  {   conn.Close()   atomic.AddUint64(&activeConnections,  ^uint64(0))  // effectively decrements the counter   }()    // Imagine complex processing here—an echo server example:   buffer  :=  make([]byte,  1024)   for  {   conn.SetDeadline(time.Now().Add(30  *  time.Second))  // prevent idle hangs   n,  err  :=  conn.Read(buffer)   if  err  !=  nil  {   log.Printf("Connection read error: %v",  err)   return   }   _,  err  =  conn.Write(buffer[:n])   if  err  !=  nil  {   log.Printf("Connection write error: %v",  err)   return   }   } }` 
```

每个连接都分配了自己的 goroutine。这种方法在低并发性下效果很好，并且很好地符合 Go 的模式。但是一旦你处理的是成千上万的连接，设计就必须考虑到系统限制。Goroutine 很便宜——但不是免费的。

### 大规模管理并发

仅接受连接是不够的；你需要控制之后发生的事情。无限制地创建 goroutine 会导致内存增长和调度器负载增加。为了保持系统稳定，并发性必须受到限制——通常使用信号量或类似的结构来限制在任何给定时间内处理活动工作的 goroutine 数量。

例如，你可能会在为每个传入连接启动新的 goroutine 之前限制同时活跃的连接数。这种策略可能涉及一个充当信号量的缓冲通道：

```go
`package  main  import  (   "net" )  var  connLimiter  =  make(chan  struct{},  10000)  // Max 10K concurrent conns  func  main()  {   ln,  _  :=  net.Listen("tcp",  ":8080")   defer  ln.Close()    for  {   conn,  _  :=  ln.Accept()    connLimiter  <-  struct{}{}  // Acquire slot   go  func(c  net.Conn)  {   defer  func()  {   c.Close()   <-connLimiter  // Release slot   }()   // Dummy echo logic   buf  :=  make([]byte,  1024)   c.Read(buf)   c.Write(buf)   }(conn)   } }` 
```

这种模式不仅有助于防止资源耗尽，而且在高负载下也能优雅地降低服务质量。根据你的硬件和工作负载特征调整这些限制是一个持续调整的过程。

信息

我们在这里使用`connLimiter`方法纯粹是为了说明目的，因为它阐明了这个想法。在现实生活中，你很可能会使用[errgroup](https://pkg.go.dev/golang.org/x/sync/errgroup)来管理 goroutine 的数量，以及一些`SIGINT`和`SIGTERM`信号处理，以实现优雅的过程终止。

### 操作系统和套接字调整

在你的 Go 应用程序能够处理超过 10,000 个并发连接之前，操作系统必须为这种规模做好准备。在 Linux 上，这通常从提高打开文件描述符的限制开始。TCP 堆栈也需要调整——默认设置通常不是为高连接负载设计的。没有这些调整，应用程序将在 Go 成为瓶颈之前很久就会遇到操作系统级别的限制。

```go
`#  Increase  file  descriptor  limit ulimit  -n  200000` 
```

但这还没有结束。你还需要：

```go
`sysctl  -w  net.core.somaxconn=65535 sysctl  -w  net.ipv4.ip_local_port_range="10000 65535" sysctl  -w  net.ipv4.tcp_tw_reuse=1 sysctl  -w  net.ipv4.tcp_fin_timeout=15` 
```

+   `net.core.somaxconn=65535`：这控制了监听套接字的挂起连接队列（回压）的大小。这里的值较小会导致当许多客户端同时尝试连接时连接丢失。

+   `net.ipv4.ip_local_port_range="10000 65535"`：定义了用于出站连接的临时端口范围。更宽的范围可以防止从同一台机器发出大量出站连接时端口耗尽。

+   `net.ipv4.tcp_tw_reuse=1`: 如果安全，允许在 `TIME_WAIT` 状态下重用套接字以建立新的连接。有助于减少套接字耗尽，尤其是在短期 TCP 连接中。

+   `net.ipv4.tcp_fin_timeout=15`: 减少了内核在连接关闭后保持套接字在 `FIN_WAIT2` 状态的时间。更短的超时时间意味着更快的资源回收，这对于每分钟有数千个套接字频繁切换的情况至关重要。

调整这些参数有助于防止操作系统成为瓶颈，随着连接数的增加。除此之外，设置如 `TCP_NODELAY` 这样的套接字选项可以通过禁用默认情况下缓冲小数据包的 [Nagle 算法](https://en.wikipedia.org/wiki/Nagle%27s_algorithm) 来减少延迟。在 Go 中，这些选项可以通过 net 包应用，或者在需要更底层控制时，通过 syscall 包直接应用。

在某些情况下，使用 Go 的 `net.ListenConfig` 允许你在创建监听器时注入自定义的套接字创建控制。这在需要设置监听器创建时的选项时特别有用：

```go
`func  main()  {   lc  :=  net.ListenConfig{   Control:  func(network,  address  string,  c  syscall.RawConn)  error  {   var  controlErr  error   err  :=  c.Control(func(fd  uintptr)  {   // Enable TCP_NODELAY on the socket   controlErr  =  syscall.SetsockoptInt(int(fd),  syscall.IPPROTO_TCP,  syscall.TCP_NODELAY,  1)   })   if  err  !=  nil  {   return  err   }   return  controlErr   },   }   listener,  err  :=  lc.Listen(context.Background(),  "tcp",  ":8080")   if  err  !=  nil  {   log.Fatalf("Error creating listener: %v",  err)   }   defer  listener.Close()   // Accept connections in a loop… }` 
```

### Go 调度器和内存压力

在纸上看起来启动 10,000 个 goroutines 可能很令人印象深刻，但重要的是这些 goroutines 的行为。如果它们大部分是空闲的——例如在网络或磁盘 I/O 上阻塞——Go 的调度器会高效地处理它们，以最小的开销进行挂起和恢复。但是，当 goroutines 积极分配内存、在紧密循环中旋转，或者不断在通道和互斥锁上竞争时，事情就会变得昂贵。你将开始看到垃圾收集压力增加和调度器抖动，这两者都会降低性能。

Go 的垃圾回收器很好地处理了短期分配，但这并非没有代价。如果你正在创建通过内存进行频繁切换的 goroutines——按请求、按消息，或者更糟糕的是按循环分配——GC 压力会迅速增加。结果不仅仅是更频繁的收集，还有更高的延迟和丢失的 CPU 周期。吞吐量下降，系统花费更多时间清理而不是进行实际工作。

为了管理这一点，你可以显式地调整 GC 的积极性：

```go
`GOGC=50` 
```

或者直接在你的代码库中：

```go
`import  "runtime/debug"  func  main()  {   debug.SetGCPercent(50)   // rest of your application logic }` 
```

`GOGC` 的默认值是 100，这意味着当堆大小与前一个 GC 周期相比翻倍时，GC 会触发。较低的值（如 50）意味着更频繁但更短的 GC 周期，以增加 CPU 开销为代价来帮助控制内存增长。

信息

在某些情况下，你可能需要相反的操作——增加 `GOGC` 值，完全关闭 GC，或者更喜欢 GOMEMLIMIT=X 和 GOGC=off 配置。**在仔细分析之前不要做出决定！**

### 优化 Goroutine 行为

考虑将你的应用程序结构化，以便 goroutines 自然阻塞，而不是积极等待或旋转。例如，而不是在紧密循环中轮询通道，使用 select 语句高效地处理：

```go
`for  {   select  {   case  msg  :=  <-msgChan:   handleMsg(msg)   case  <-ctx.Done():   return   } }` 
```

如果 goroutines 必须等待，则优先选择在 Go 提供的通道或同步原语上阻塞，如互斥锁或条件变量，而不是积极轮询。

### 对象池化和复用

另一种减少内存分配和 GC 开销的关键技术[是使用`sync.Pool`]：

```go
`var  bufPool  =  sync.Pool{   New:  func()  any  {  return  make([]byte,  1024)  }, }  func  handleRequest()  {   buf  :=  bufPool.Get().([]byte)   defer  bufPool.Put(buf)  // (1)    // use buffer for request handling }` 
```

1.  注意这里！它严格依赖于工作流程，当你必须将对象返回到池中时！

通过池化复用对象减少内存波动。由于分配较少，垃圾收集器运行得更少，影响也更小。这直接转化为更低的延迟和更可预测的性能：

### 连接生命周期管理

连接不仅仅是被接受然后被遗忘——它经历了一个完整的生命周期：设置、数据交换、拆除。问题通常出现在安静阶段。未清理的空闲连接可能会占用内存并无限期地阻塞 goroutines。强制执行读写截止日期是至关重要的。心跳消息也有帮助——它们为你提供了一种在不等待操作系统超时的情况下检测死对等体的方法。

在一个生产案例中，客户端响应缓慢导致 goroutines 在读取操作中被阻塞。随着时间的推移，这种情况逐渐累积，直到系统开始退化。添加截止日期和轻量级健康检查修复了泄漏问题。goroutines 不再滞留，资源使用在负载下保持平稳。

每个连接仍然在自己的 goroutine 中运行——但是有了适当的生命周期管理，扩展并不以稳定性为代价。

```go
`for  {   conn,  err  :=  ln.Accept()   if  err  !=  nil  {   // handle error   }   go  handle(conn) }` 
```

在处理程序内部，使用计时器每几秒钟触发一次，触发定期心跳以保持连接活跃和响应：

```go
`ticker  :=  time.NewTicker(5  *  time.Second) defer  ticker.Stop()` 
```

在从客户端读取之前，服务器设置了一个读取截止日期——如果在那个时间内没有接收到数据，操作将失败，连接将被清理。这防止了阻塞读取无限期地阻塞 goroutine：

```go
`conn.SetReadDeadline(time.Now().Add(10  *  time.Second)) _,  err  :=  reader.ReadString(' ') if  err  !=  nil  {   return  // read timeout or client gone }` 
```

同样，在发送心跳之前，服务器设置了一个写入截止日期。如果客户端无响应或网络缓慢，写入将立即失败，避免资源泄漏：

```go
`select  { case  <-ticker.C:   conn.SetWriteDeadline(time.Now().Add(10  *  time.Second))   conn.Write([]byte("ping")) default:   // skip heartbeat if not due }` 
```

循环处理传入的消息并定期发送心跳，读写截止日期在双方都设定了边界。这种设置使每个连接都处于活跃监督之下。静默故障不会滞留，系统避免了以稳定性换取性能。

## 实际调优和扩展陷阱

扩展到 10K+个连接不仅仅是代码问题——它需要预测和缓解堆栈多层中的潜在陷阱。除了解决内存占用、文件描述符限制和阻塞 I/O 之外，一系列高并发回声服务器测试在真实负载下揭示了额外的性能考虑因素。

一个实验从一个简单的基于行的回声服务器开始。基线处理程序很简单：

```go
`func  handle(conn  net.Conn)  {   defer  conn.Close()   reader  :=  bufio.NewReader(conn)    for  {   line,  err  :=  reader.ReadString('\n')   if  err  !=  nil  {   fmt.Printf("Connection closed: %v\n",  err)   return   }   conn.Write([]byte(line))  // echo   } }` 
```

使用像`tcpkali`这样的工具：

```go
`tcpkali  -m  $'ping\n'  -c  10000  --connect-rate=2000  --duration=60s  127.0.0.1:9000` 
```

测试将并发连接数提升至 10,000 个。在 60 秒的运行过程中，它发送了 2.4 MiB 的数据，接收了 210.3 MiB 的数据。每个连接的平均速度约为 0.4 kBps，总吞吐量为 29.40 Mbps 下行和 0.33 Mbps 上行。这一结果突显了服务器在高并发持续压力下对外出数据的有限响应能力，`fd.Read`上存在大量背压。

### 服务器监控和基准测试

信息

我们使用`c5.2xlarge`（8 CPU，16 GiB）的 AWS 实例进行所有这些测试。

为了更好地理解系统在高负载下的行为，启用了 Go 内置的跟踪功能：

```go
`import  (   "runtime/trace"   "os"   "log" )  func  main()  {   f,  err  :=  os.Create("trace.out")   if  err  !=  nil  {  log.Fatal(err)  }   defer  f.Close()    trace.Start(f)   defer  trace.Stop()    // server logic ... }` 
```

在运行服务器并收集跟踪后，命令

```go
`go  tool  trace  trace.out` 
```

发现运行时的大部分时间被阻塞在`fd.Read`和`fd.Write`中，这表明有机会更有效地平衡 I/O 操作。跟踪分析显示，`fd.Read`占运行时的 23%，而`fd.Write`消耗了 75%，表明在回显期间存在显著的写端背压。尽管`ulimit -n`被设置为 65535（AWS EC2 实例的硬限制），但由于 I/O 阻塞和临时端口范围限制，系统仍然遇到了瓶颈。

### 通过缓冲写入减少写阻塞

将连接写入包裹在`bufio.Writer`中，并定期刷新，而不是每次写入后刷新。更新的代码片段：

```go
`reader  :=  bufio.NewReader(conn) writer  :=  bufio.NewWriter(conn) count  :=  0 const  flushInterval  =  10  for  {   line,  err  :=  reader.ReadString('\n')   if  err  !=  nil  {   return   }   writer.WriteString(line)   count++   if  count  >=  flushInterval  {   writer.Flush()   count  =  0   } }` 
```

使用以下基准测试：

```go
`tcpkali  -m  $'ping\n'  -c  10000  --connect-rate=2000  --duration=60s  127.0.0.1:9000` 
```

显示了显著的改进——吞吐量从大约 33.8 MiB 增加到超过 1661 MiB 接收和 1369 MiB 发送，在 10,000 个连接中，每个连接的带宽达到 5.3 kBps。总吞吐量上升到 232.28 Mbps 下行和 191.41 Mbps 上行。跟踪配置文件确认了在更重的并发负载下，I/O 等待时间更加平衡。

### 处理突发负载和 CPU 密集型工作负载

为了评估服务器在极端连接压力下的行为，执行了一个突发测试，以每秒 5,000 个连接的速度将连接数提升至 30,000 个：

```go
`tcpkali  -m  $'ping\n'  -c  30000  --connect-rate=5000  --duration=60s  127.0.0.1:9000` 
```

服务器干净利落地将并发连接数提升至 30,000 个，并持续了整整 60 秒。它处理了总共 2580.3 MiB 发送和 1250.9 MiB 接收的数据，保持了 360.75 Mbps 的上行总吞吐量和 174.89 Mbps 的下行总吞吐量。每通道带宽自然下降到大约 1.2 kBps，但所有通道的稳定性以及未丢失的连接表明，即使在规模扩大时，也能有效地分配负载和稳健地处理 I/O。

为了模拟 CPU 密集型工作负载，服务器被修改为对每行输入计算 SHA256 哈希：

```go
`func  hash(s  string)  string  {   h  :=  sha256.Sum256([]byte(s))   return  hex.EncodeToString(h[:]) }  ...  for  {   line,  err  :=  reader.ReadString('\n')   if  err  !=  nil  {   return   }   _  =  hash(line)  // simulate CPU-intensive processing   writer.WriteString(line)   count++   if  count  >=  flushInterval  {   writer.Flush()   count  =  0   } }` 
```

在此配置中，使用相同的 30,000 个连接设置，吞吐量下降到 1068.3 MiB 发送和 799.3 MiB 接收。聚合带宽下降到上游 149.35 Mbps 和下游 111.74 Mbps，每连接带宽下降到大约 0.7 kBps。虽然服务器保持了完整的连接数和正常运行时间，但跟踪分析揭示了在 runtime.systemstack_switch 和 GC 相关函数中花费的时间增加。这清楚地展示了计算密集型任务对整体吞吐量的影响，并加强了在高并发操作时在 I/O 和 CPU 工作负载之间进行仔细平衡的必要性。

### 总结技术收益

在四个不同的服务器配置中进行基准测试，揭示了缓冲、并发扩展和 CPU 密集型任务如何在负载下影响性能：

| 功能 | 基准（10K，无缓冲） | 10K 缓冲连接 | 30K 缓冲连接 | 30K + CPU 负载（SHA256） |
| --- | --- | --- | --- | --- |
| 处理的连接数 | 10,000 | 10,000 | 30,000 | 30,000 |
| 发送的数据（60 秒） | 2.4 MiB | 1369.1 MiB | 2580.3 MiB | 1068.3 MiB |
| 接收到的数据（60 秒） | 210.3 MiB | 1661.4 MiB | 1250.9 MiB | 799.3 MiB |
| 每通道带宽 | ~0.4 kBps | ~5.3 kBps | ~1.2 kBps | ~0.7 kBps |
| 聚合带宽（↓/↑） | 29.40 / 0.33 Mbps | 232.28 / 191.41 Mbps | 174.89 / 360.75 Mbps | 111.74 / 149.35 Mbps |
| 数据包速率估计（↓/↑） | 329K / 29 数据包/秒 | 278K / 16K 数据包/秒 | 135K / 32K 数据包/秒 | 136K / 13K 数据包/秒 |
| I/O 特性 | 严重的写回压 | 读写平衡 | 规模下高效 | 来自 CPU 竞争的延迟 |
| CPU 和 GC 压力 | 低 | 低 | 中等 | 高（GC + 哈希计算） |

从 10,000 个无缓冲连接的基准开始，服务器显示的吞吐量有限——在 60 秒内仅发送了 2.4 MiB，接收了 210.3 MiB，并显示出明显的写回压迹象。引入相同连接数的缓冲写入解锁了超过 1369 MiB 的发送和 1661 MiB 的接收，将吞吐量提高了超过一个数量级，并平衡了 I/O 等待时间。进一步扩展到 30,000 个连接保持了稳定性并提高了整体吞吐量，尽管每个连接的带宽有所减少。当每条消息添加 SHA256 哈希时，总吞吐量显著下降，证实了预期的 CPU 瓶颈，并加强了在设计高并发、I/O 密集型服务时考虑计算延迟的必要性。

这些配置文件作为性能感知开发的实际参考，其中传输、内存和计算必须协同优化以实现现实世界的可扩展性。

正如您所看到的，即使实现 30,000 个并发连接并保持可靠性能也是一个非同小可的任务。测试结果证明，一旦工作负载偏离了简单的回声服务器——例如，通过添加日志记录、CPU 密集型处理或更复杂的读写逻辑——吞吐量和稳定性会迅速下降。在规模上的性能高度依赖于工作流程特征，如 I/O 模式、同步频率和内存压力。

综合来看，这些测试强调了在构建高性能、可扩展的网络系统时进行工作负载感知调优和平台特定调整的必要性。
