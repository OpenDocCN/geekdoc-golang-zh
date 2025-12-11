# Go 网络内部结构

> 原文：[`goperf.dev/02-networking/networking-internals/`](https://goperf.dev/02-networking/networking-internals/)

Go 的网络模型表面上看似简单——启动一个 goroutine，接受连接，从中读取，并写入响应。但在这看似简单的背后，是一个高度优化和精细调校的运行时，它以最小的操作系统开销处理成千上万的连接。在这篇深入探讨中，我们将了解使这一切成为可能机制：从 goroutine 和调度器到 Go 如何与操作系统级别的轮询器（如 `epoll`、`kqueue` 和 IOCP）交互。

## Goroutines 和运行时调度器

Goroutines 是由 Go 运行时管理的轻量级用户空间线程。它们易于创建（只有几 KB 的堆栈），可以扩展到数百万。但它们并非魔法——它们依赖于运行时调度器来在有限的操作系统线程之间多路复用执行。

Go 的调度器基于 M:N 模型：

+   **M (Machine)**：表示一个操作系统线程。

+   **G (Goroutine)**：表示实际的任务或协程。

+   **P (Processor)**：表示调度上下文（持有运行队列、缓存）。

每个 P 可以使用一个 M 来执行一个 G。P 的数量与 GOMAXPROCS 相同。如果一个 goroutine 在 I/O 上阻塞，另一个可运行的 G 可以暂停并重用线程。

```go
stateDiagram-v2
    [*] --> New : goroutine declared
    New --> Runnable : go func() invoked
    Runnable --> Running : scheduled on an available P
    Running --> Waiting : blocking syscall, channel op, etc.
    Waiting --> Runnable : event ready, rescheduled
    Running --> Terminated : function exits or panics
    Waiting --> Terminated : canceled or panicked
    Terminated --> [*]

    state "Go Scheduler\n(GOMAXPROCS = N)" as Scheduler {
        [*] --> P1
        [*] --> P2
        ...
        [*] --> PN

        P1 --> ScheduleGoroutine1 : pick from global/runq
        P2 --> ScheduleGoroutine2
        PN --> ScheduleGoroutineN
    }

    note right of Runnable
        Ps (Processors) pick Runnable goroutines
        based on availability up to GOMAXPROCS
    end note

    note right of Scheduler
        GOMAXPROCS determines how many Ps
        can execute goroutines in parallel.
    end note
```

## Goroutine 中的阻塞 I/O：实际上发生了什么？

假设一个 goroutine 调用 `conn.Read()`。这看起来像是阻塞的——但仅从 goroutine 的角度来看。内部，Go 的运行时拦截调用并使用一种称为 [netpoller](https://go.dev/src/runtime/netpoll.go) 的机制。

在基于 Unix 的系统上，Go 使用基于就绪的轮询（Linux 上的 `epoll`，macOS/BSD 上的 `kqueue`）。当一个 goroutine 执行 `read(fd)` 这样的系统调用时，运行时会检查文件描述符是否就绪。如果不是：

1.  goroutine 被挂起。

1.  文件描述符已注册到轮询器。

1.  操作系统线程被释放以运行其他工作。

1.  当 fd 准备就绪时，轮询器唤醒，运行时将 goroutine 标记为可运行。

```go
flowchart TD
    A["Goroutine: conn.Read()"] --> B[netpoller checks FD]
    B --> C{FD ready?}
    C -- No --> D[Park goroutine]
    D --> E[FD registered with epoll]
    E --> F[epoll_wait blocks]
    F --> G[FD ready]
    G --> H[Wake goroutine]
    H --> I[Re-schedule]
    C -- Yes --> H
```

该系统使 Go 能够使用少量线程并发地为大量客户端提供服务，避免了传统线程-连接模型的开销。

## `net` 包的内部结构

让我们看看 `net.Listen("tcp", ":8080")` 和 `conn.Read()` 背后发生了什么。

+   `net.Listen` 调用 `net.ListenTCP`，它构建了一个包含套接字的 `netFD` 结构体。

+   通过 `syscall.SetNonblock(fd, true)` 将套接字标记为非阻塞。

+   `netFD` 上的 `Accept` 和 `Read` 方法建立在系统调用之上，但通过内部轮询器路由，并用逻辑包装以产生和恢复 goroutine。

这是一张调用链的大致图：

```go
flowchart TD
    A[net.Listen] --> B[ListenTCP] --> C[listenFD]
    C --> D["pollDesc (register with netpoll)"]
    D --> E[runtime-integrated non-blocking syscall wrappers]
```

这种架构使得从开发者的角度来看，阻塞调用转换为与内核的非阻塞交互。

## Netpoller：使用 Epoll/Kqueue/IOCP 进行轮询

**netpoller** 是一个运行时子系统，它将低级轮询机制与 Go 的调度系统集成。每个 fd 都有一个关联的 `pollDesc`，这有助于协调 goroutine 的挂起和恢复。

poller 在一个专用线程（或线程）中运行，该线程循环遍历 OS 等待原语：

+   [epoll_wait](https://man7.org/linux/man-pages/man2/epoll_wait.2.html) (Linux)

+   [kqueue](https://en.wikipedia.org/wiki/Kqueue) (macOS/BSD)

+   [IOCP](https://learn.microsoft.com/en-gb/windows/win32/fileio/i-o-completion-ports) (Windows)

当 I/O 事件触发时，poller 找到相关的 `pollDesc`，识别已停泊的 goroutine，并将其放回运行队列。

在 Go 源代码中，相关的文件包括：

+   [runtime/netpoll_epoll.go](https://go.dev/src/runtime/netpoll_epoll.go)

+   [runtime/netpoll_kqueue.go](https://go.dev/src/runtime/netpoll_kqueue.go)

+   [runtime/netpoll_windows.go](https://go.dev/src/runtime/netpoll_windows.go)

Go 的 poller 是基于就绪状态的（不是基于完成状态的，除了 Windows IOCP）。它处理：

+   fd 注册

+   在就绪时唤醒 goroutine

+   与运行队列（P-局部或全局）集成

## 示例：高性能 TCP Echo 服务器

让我们分解一个简单的 Go TCP echo 服务器，并将每个部分映射到 Go 的内部网络和调度机制——包括 `netFD`、`poll.FD` 和 goroutines。

<details class="example"><summary>简单的 Echo 服务器源代码</summary>

```go
`package  main  import  (   "bufio"   "fmt"   "net"   "time" )  func  main()  {   // Start listening on TCP port 9000   listener,  err  :=  net.Listen("tcp",  ":9000")   if  err  !=  nil  {   panic(err)  // Exit if the port can't be bound   }   fmt.Println("Echo server listening on :9000")    // Accept incoming connections in a loop   for  {   conn,  err  :=  listener.Accept()  // Accept new client connection   if  err  !=  nil  {   fmt.Printf("Accept error: %v\n",  err)   continue  // Skip this iteration on error   }    // Handle the connection in a new goroutine for concurrency   go  handle(conn)   } }  // handle echoes data back to the client line-by-line func  handle(conn  net.Conn)  {   defer  conn.Close()  // Ensure connection is closed on exit    reader  :=  bufio.NewReader(conn)  // Wrap connection with buffered reader    for  {   // Set a read deadline to avoid hanging goroutines if client disappears   conn.SetReadDeadline(time.Now().Add(5  *  60  *  time.Second))  // 5 minutes timeout    // Read input until newline character   line,  err  :=  reader.ReadString('\n')   if  err  !=  nil  {   fmt.Printf("Connection closed: %v\n",  err)   return  // Exit on read error (e.g. client disconnect)   }    // Echo the received line back to the client   _,  err  =  conn.Write([]byte(line))   if  err  !=  nil  {   fmt.Printf("Write error: %v\n",  err)   return  // Exit on write error   }   } }` 
```</details>

### 导入和设置

```go
`import  (   "bufio"   "fmt"   "net"   "time"   "sync/atomic" )` 
```

**涉及内部**:

+   `net` 包抽象了系统级网络。

+   在底层：

    +   使用 `netFD`（内部，私有结构）

    +   包装 `poll.FD` 以实现非阻塞 I/O

    +   使用操作系统功能如 `epoll`、`kqueue` 或 `IOCP` 进行事件通知

### 监听器设置

```go
`listener,  err  :=  net.Listen("tcp",  ":9000") if  err  !=  nil  {   panic(err) } fmt.Println("Echo server listening on :9000")` 
```

**涉及内部**:

+   `net.Listen()` 返回一个 `TCPListener`

    +   内部调用 `syscall.socket`、`bind`、`listen`

    +   将 `netFD` 与套接字关联

+   监听器使用 Go 的内部 poller 来启用非阻塞 `Accept`

### 接受循环和 goroutine 调度

```go
`for  {   conn,  err  :=  listener.Accept()   if  err  !=  nil  {   fmt.Printf("Accept error: %v\n",  err)   continue   }   go  handle(conn) }` 
```

**涉及内部**:

+   `listener.Accept()` → `netFD.Accept()` → `poll.FD.Accept()` → `syscall.accept`

    +   非阻塞，通过 Go 的 poller (`runtime_pollWait`) 等待

+   `go handle(conn)` 启动一个 **goroutine (G**)

    +   调度到 **P**（处理器）

    +   `P` 是 Go 的 M:N 调度器的一部分，由 `GOMAXPROCS` 管理

### 连接处理器

```go
`func  handle(conn  net.Conn)  {   defer  conn.Close()    reader  :=  bufio.NewReader(conn)    for  {   conn.SetReadDeadline(time.Now().Add(5  *  60  *  time.Second))    line,  err  :=  reader.ReadString('\n')   if  err  !=  nil  {   fmt.Printf("Connection closed: %v\n",  err)   return   }    _,  err  =  conn.Write([]byte(line))   if  err  !=  nil  {   fmt.Printf("Write error: %v\n",  err)   return   }   } }` 
```

**涉及内部**:

+   `bufio.NewReader(conn)` 包装 `net.Conn`，它由 `*TCPConn` 和 `netFD` 支持。

+   `ReadString()` 在底层调用 `conn.Read()`：

    +   `netFD.Read()` → `poll.FD.Read()` → `syscall.Read()`

    +   使用 `runtime_pollWait` 在数据未就绪时让出 goroutine

+   `SetReadDeadline` 通过与运行时的网络 poller 集成来设置超时，以防止无限期阻塞。

+   `conn.Write()` → `netFD.Write()` → `poll.FD.Write()` → `syscall.write`

### 内部流程图

```go
sequenceDiagram
    participant L as Listener Goroutine
    participant N as netFD
    participant P as Go Poller
    participant S as syscall layer
    participant H as Handler Goroutine

    L->>N: Accept()
    N->>P: Wait for connection (runtime_pollWait)
    P->>S: syscall.accept
    S-->>L: Return net.Conn
    L->>H: go handle(conn)

    H->>N: Read()
    N->>P: Wait for data (runtime_pollWait)
    P->>S: syscall.read
    S-->>H: Return data
    H->>N: Write()
    N->>P: Check readiness
    P->>S: syscall.write
    S-->>H: Confirm write
```

只要满足以下条件，此模型就可以很好地扩展：

+   确保你的 `ulimit -n` 足够高

+   避免共享状态和竞争

+   为你的工作负载调整 GOMAXPROCS

## 规模观察

随着连接数的增加（在此处查看可能的样子)：

+   每个连接的内存和垃圾回收压力增加

+   频繁的 goroutine 上下文切换可能会引入延迟

+   协调通道、超时和背压增加了复杂性

一些缓解策略：

+   使用 `sync.Pool` 进行缓冲区重用

+   最小化垃圾回收暂停（避免每个请求的分配）

+   优先选择对 `netpoll` 友好的设计（避免长时间占用 CPU 的 goroutine）

* * *

Go 的模型以用户空间调度和事件驱动 I/O 协调来交换 OS 级别的多路复用。它不是万能的银弹——但使用得当，它提供了一个构建可扩展网络服务的强大平台。理解这些内部机制有助于你避免常见陷阱，在正确的层级进行优化，并构建在负载下表现可预测的系统。
