# 低级网络优化：重要的套接字选项

> 原文：[https://goperf.dev/02-networking/low-level-optimizations/](https://goperf.dev/02-networking/low-level-optimizations/)

当系统负载时，套接字设置可以限制吞吐量和延迟。默认设置是为了安全性和兼容性，而不是针对任何特定的工作负载。在实践中，它们通常在 CPU 或内存成为瓶颈之前成为瓶颈。Go 允许你通过 `syscall` 访问底层文件描述符，因此你可以更改关键的套接字选项，而无需放弃其并发模型或标准库。

## 禁用 Nagle 算法：`TCP_NODELAY`

Nagle 算法存在是为了使 TCP 更高效。你发送的每个小数据包都携带了大量的头部信息，如果未经检查，这些头部信息会浪费很多带宽。Nagle 通过延迟小写入，直到可以将它们批量成完整的数据段，从而减少了开销和网络嘈杂。这种权衡——以延迟为代价换取带宽——通常是可接受的，这也是为什么它是默认开启的。但是，如果你的应用程序发送大量的小、时间敏感的消息，如游戏服务器或交易系统，等待缓冲区填满甚至几毫秒都可能造成伤害。

```go
sequenceDiagram
    participant App as Application
    participant TCP as TCP Stack
    participant Net as Network

    App->>TCP: Send 1 byte
    note right of TCP: Buffering (no ACK received)
    App->>TCP: Send 1 byte
    note right of TCP: Still buffering...

    TCP-->>Net: Send 2 bytes (batched)
    Net-->>TCP: ACK received

    App->>TCP: Send 1 byte
    TCP-->>Net: Immediately send (ACK received, buffer clear)
```

Nagle 算法通过延迟发送小数据包，直到有更多数据要发送或收到确认，以换取效率。这种延迟对于大量传输来说是可接受的，但对于需要快速、小消息的任何应用来说都是一个问题。设置 `TCP_NODELAY` 可以关闭它，使数据立即发送出去。这对于游戏、交易、实时视频和其他交互式系统来说至关重要，在这些系统中，等待是不可接受的。

在 Go 中，你可以使用 `TCP_NODELAY` 关闭 Nagle 算法：

```go
func SetTCPNoDelay(conn *net.TCPConn) error {
    return conn.SetNoDelay(true)
} 
```

## `SO_REUSEPORT` 用于可伸缩性

`SO_REUSEPORT` 允许同一台机器上的多个套接字绑定到相同的端口，并同时接受连接。内核将新连接分配到所有套接字上，而不是将所有传入连接都通过一个套接字，这样每个套接字都能获得自己的一份负载。这在运行多个独立接受连接的工作进程或线程时非常有用，因为它消除了用户空间协调的需求，避免了单个接受队列上的竞争。它还通过让每个进程或线程直接处理自己的连接队列，更好地利用了多个 CPU 核心。

`SO_REUSEPORT` 的典型场景：

+   高性能的 Web 服务器，其中多个工作进程在相同的端口上调用 bind()。内核将传入连接请求分配到所有已绑定套接字的 accept() 队列中，通常使用 4 元组的哈希或轮询，消除了用户空间调度器的需求。

+   多线程或多进程服务器可以并行接受连接。当与 Go 的 GOMAXPROCS 结合使用时，每个线程或进程都可以独立在其自己的文件描述符上调用 accept()，从而避免了单个队列上的锁定竞争，并充分利用了所有 CPU 核心。

+   具有多个进程绑定到同一端口以提高弹性的容错设计。如果一个进程退出或被杀死，其他进程将继续服务连接而不会中断，因为每个进程都维护自己的独立 accept() 队列。

在 Go 中，`SO_REUSEPORT` 并未在标准库中公开，但在创建套接字时可以通过 syscall 设置。这是通过 `syscall.SetsockoptInt` 实现的，它操作套接字的文件描述符。您传递协议级别（`SOL_SOCKET`）、选项（`SO_REUSEPORT`）和值（`1` 以启用）。这必须在调用 `bind()` 之前完成，因此通常放置在 `net.ListenConfig` 的控制回调中，该回调在套接字绑定之前运行。

```go
listenerConfig := &net.ListenConfig{
    Control: func(network, address string, c syscall.RawConn) error {
        return c.Control(func(fd uintptr) {
            syscall.SetsockoptInt(int(fd), syscall.SOL_SOCKET, syscall.SO_REUSEPORT, 1)
        })
    },
}
listener, err := listenerConfig.Listen(context.Background(), "tcp", ":8080") 
```

## 调整套接字缓冲区大小：`SO_RCVBUF` 和 `SO_SNDBUF`

套接字缓冲区大小——`SO_RCVBUF` 用于接收和 `SO_SNDBUF` 用于发送——直接影响吞吐量和系统调用次数。这些缓冲区在内核中持有传入和传出数据，平滑突增，并允许应用程序以自己的速度读取和写入。

当缓冲区过小时，它们会很快填满，内核会不断唤醒应用程序来读取或写入数据。这种额外的处理增加了 CPU 使用率，并限制了可以通过连接推送的数据量。如果缓冲区过大，它们只是浪费内存，并让数据包在队列中堆积的时间比所需的时间更长，这会增加延迟并损害系统繁忙时的响应性。关键是要使缓冲区足够大，以保持链路忙碌，但不要大到它们变成积压。

这就是调整缓冲区的方法：

```go
func SetSocketBuffers(conn *net.TCPConn, recvBuf, sendBuf int) error {
    if err := conn.SetReadBuffer(recvBuf); err != nil {
        return err
    }
    return conn.SetWriteBuffer(sendBuf)
} 
```

在 Linux 系统上，`SO_RCVBUF` 和 `SO_SNDBUF` 的典型默认大小从 128 KB 到 256 KB 不等，这取决于内核版本和系统配置。这些默认值的选择是为了在最小化延迟和避免过度使用内存之间取得平衡。对于高带宽应用或具有高延迟的连接（例如，长途 TCP 连接），增加这些缓冲区大小——有时达到几个兆字节——可以通过允许更多的飞行数据而不阻塞来显著提高吞吐量。

正确的缓冲区大小取决于 RTT、链路带宽以及应用程序发送的消息大小。一个常见的经验法则是将缓冲区匹配到带宽-延迟积（BDP），即 `带宽 × RTT`。这样，连接可以保持管道满载而不停滞。

为了确定正确的缓冲区大小，您需要在实际负载下进行测试。iperf3 等工具适用于测量原始吞吐量，而特定于应用程序的剖析（pprof、netstat、自定义指标）有助于发现实际卡住的地方。在负载测试期间逐步增加缓冲区大小，并观察收益何时趋于平稳——这通常是一个停止的好地方。

警告

最佳设置取决于您的系统实际运行的方式，因此您需要在负载下进行测量。猜测或从其他地方复制值通常不起作用——您必须测试和调整，直到它以您需要的方式运行。

## TCP Keepalives for Reliability

TCP keepalive 探测可以检测死对等连接，及时释放资源。Keepalives 可以防止在长时间运行的服务中挂起连接。

大多数平台上的默认 keepalive 设置都很保守——空闲时间为 2 小时，每 75 秒进行 10 次探测——旨在用于通用环境。对于需要更快故障检测的服务器端应用程序（例如，反向代理、通过不可靠链路的微服务），通常更激进的设置更为常见：30 秒空闲，每 10 秒进行 3 次探测。

然而，激进的调整会增加流量并增加在拥塞链路上的误报风险。一种推荐的方法是平衡早期检测和网络条件。典型的调整值——空闲 30-60 秒，间隔 10-15 秒，探测 3-5 次——并非由任何 RFC 或标准指定，而是来自操作实践和供应商指南。例如，PostgreSQL、Kubernetes 和 AWS 都推荐这个范围内的值，以与云负载均衡器超时和服务 SLA 保持一致。这些数字是从经验中得出的，旨在最大限度地减少误报和长检测延迟，正如在 PostgreSQL 文档、AWS 网络博客和 Kubernetes 问题讨论中讨论的那样。

这些设置可以通过 `sysctl` 在系统范围内调整，或在某些平台（例如 Linux）上通过平台特定的套接字选项或暴露 `TCP_KEEPIDLE`、`TCP_KEEPINTVL` 和 `TCP_KEEPCNT` 的库按连接进行调整。在 Go 中，`SetKeepAlivePeriod` 只控制空闲时间；更深入的调整可能需要 cgo 或原始系统调用。

在 Go 中启用和调整 keepalives：

```go
func SetKeepAlive(conn *net.TCPConn, idleTime time.Duration) error {
    if err := conn.SetKeepAlive(true); err != nil {
        return err
    }
    return conn.SetKeepAlivePeriod(idleTime)
} 
```

Go 中的 `SetKeepAlivePeriod` 方法仅控制发送第一个 keepalive 探测之前的空闲时间。在 Linux 上，这对应于 `TCP_KEEPIDLE` 参数；在 macOS 和 BSD 上，它映射到 `TCP_KEEPALIVE`。然而，它不影响后续探测之间的间隔（`TCP_KEEPINTVL`）或允许的失败探测次数（`TCP_KEEPCNT`）。如果需要更细粒度的控制，必须单独使用原始系统调用或 cgo 绑定来设置这两个值。如果没有调整 `KEEPINTVL` 和 `KEEPCNT`，即使 `SetKeepAlivePeriod` 设置较低，死连接也可能需要几分钟才能检测到。

警告

在没有适当测试的情况下不要更改默认值。正确的设置完全取决于你的工作负载、网络路径以及应用程序在实际条件下的行为。不同的配置可以产生非常不同的结果，而在其他地方有效的东西在这里可能会造成伤害。你将找到许多带有建议数字的博客文章和建议，但那些只是起点。确定你环境中什么有效唯一可靠的方法是亲自测试和测量。

## 连接队列：SOMAXCONN

连接队列（`SOMAXCONN`）定义了可以排队等待接受多少个挂起的连接。当队列填满时，内核会拒绝额外的连接尝试（通常导致`ECONNREFUSED`(1)或丢弃SYN数据包），直到空间变得可用，这可能导致在高负载下客户端看到连接错误。

1.  在流套接字上的`connect()`调用没有在远程地址上找到监听者。有关更多详细信息，请参阅Linux [手册页](https://man7.org/linux/man-pages/man2/connect.2.html)。

Linux上`SOMAXCONN`的默认值通常是128或4096，具体取决于内核版本和发行版。这些默认值是为了在内存使用和处理正常连接速率之间取得平衡而选择的，但它们对于高流量服务器或经历连接突发的服务可能太低。

当服务器受到连接突发（如故障转移后或高峰期间）的冲击时，更大的队列有助于避免客户端立即掉线。提高`SOMAXCONN`允许内核在应用程序赶上并保持服务在负载下可达的同时排队更多连接。

修改`SOMAXCONN`是一个系统级设置。在Linux上，你通常通过内核参数进行调整。

```go
# System-level setting
sysctl -w net.core.somaxconn=4096 
```

## 在Go中安全地包装系统调用

通过系统调用与套接字选项一起工作意味着直接处理文件描述符。这些调用必须在套接字绑定或使用之前发生，这使得时机很重要，并且很容易出错。如果你设置选项太晚，内核会忽略它，或者更糟糕的是，你会得到难以复现的错误。由于你绕过了Go运行时，你也负责检查错误并确保文件描述符保持有效状态。`syscall.RawConn`存在就是为了帮助解决这个问题——它为你提供了一个受控的钩子，可以在设置过程中在精确的点上运行你的代码。

```go
listenerConfig := net.ListenConfig{
    Control: func(network, address string, c syscall.RawConn) error {
        return c.Control(func(fd uintptr) {
            // Custom syscall here; safely handled means it executes within the correct context,
            // before the socket is handed over to the Go runtime, with proper error checking and synchronization
        })
    },
} 
```

网络套接字选项在创建时、在正确的时间和位置设置。这避免了它们被遗忘或错误应用。 

## 实际考虑因素

调整套接字总是一种权衡——较低的延迟、较高的吞吐量、更好的可靠性和合理的资源使用通常不会免费地同时出现。你需要了解你的工作负载，一次改变一件事，并在实际负载下进行测试。没有监控和仪表，你只是在猜测，所以测量一切并确保这些更改确实有帮助。
