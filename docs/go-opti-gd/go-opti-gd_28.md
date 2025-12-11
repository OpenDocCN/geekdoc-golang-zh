# 在 Go 中使用 QUIC：使用 quic-go 构建低延迟服务

> 原文：[`goperf.dev/02-networking/quic-in-go/`](https://goperf.dev/02-networking/quic-in-go/)

QUIC 已成为一种健壮的协议，解决了传统 TCP 连接的许多固有局限性。QUIC 将加密、多路复用和连接迁移结合到一个统一的协议中，旨在优化网络性能，尤其是在实时和以移动优先的应用程序中。在 Go 中，[quic-go](https://github.com/quic-go/quic-go)是主要的 QUIC 实现，并作为构建内置加密和流多路复用的高效、低延迟网络服务的实用基础。

## 理解 QUIC

QUIC 最初在 Google 开发，后来由 IETF 标准化，它重新思考了传输层以克服 TCP 的长期限制：

+   **头部阻塞**：TCP 交付一个有序的字节流，因此包丢失会阻止其后的所有内容。QUIC 将数据分成独立的流，即使其中一个延迟，其他流也可以继续进行。

+   **按包加密和头部保护**：QUIC 在包级别应用加密，包括与包号相关的选择性头部保护——这是 DTLS 基于记录的帧格式所不能支持的。

+   **内置传输机制**：QUIC 将流多路复用、流量控制和重传作为协议的一部分来处理。相比之下，DTLS 仅保护数据报，并将可靠性和排序留给应用程序。

+   **连接 ID 抽象**：QUIC 使用连接 ID 而不是 IP 和端口号元组来标识会话，允许连接在网络变化中持续存在。DTLS 没有提供这种抽象，使得移动性难以实现。

### QUIC 与 TCP：关键差异

QUIC 与 TCP 采取了根本不同的方法。虽然 TCP 直接建立在 IP 之上，并且需要在数据流之前进行连接相关的握手，但 QUIC 在 UDP 之上运行并处理自己的连接逻辑，减少了设置开销并提高了启动延迟。这种架构选择使得 QUIC 能够提供多路复用、独立的流，有效地消除了 TCP 中常见的头部阻塞问题，即一个包的延迟或丢失会阻止后续包。

QUIC 直接将其传输层与 TLS 1.3 集成，消除了 TCP+TLS 中看到的分层协商。这种设计简化了握手过程，并实现了 0-RTT 数据，即重复连接可以立即开始传输加密有效载荷——这是 TCP 所不支持的功能。

另一个关键的区别是连接的识别方式。TCP 连接绑定到特定的 IP 和端口号，因此任何网络接口的变化都会导致连接中断。QUIC 通过使用在地址变化中保持稳定的连接 ID 来避免这种情况，允许设备在网络之间移动时会话可以无缝继续——这对于移动和延迟敏感用例至关重要。

## QUIC 是基于 DTLS 的吗？

虽然 QUIC 和 DTLS 都在 UDP 上使用 TLS 密码学原语，但 QUIC**并不基于 DTLS 构建**。相反，QUIC 直接将 TLS 1.3 集成到其传输层中，仅继承了加密握手——而不是 DTLS 的记录帧或协议结构。

QUIC 定义了自己的数据包编码、多路复用、重传和加密格式。它将 TLS 握手消息封装在 QUIC 数据包中，并将加密状态与传输功能（如数据包编号和流 ID）紧密耦合。相比之下，DTLS 作为安全数据报层在 UDP 上运行，提供加密和认证，但将传输语义（如重传、排序或流量控制）留给高层。

QUIC 拒绝 DTLS 作为其安全基础的原因包括：

+   **握手和传输的更紧密集成**：QUIC 将 TLS 协商与传输状态设置合并，使得 0-RTT 重用和 1-RTT 设置在更少的往返中完成。DTLS 的分层模型引入了更高的延迟。

+   **细粒度的加密控制**：QUIC 对每个数据包的头部和有效载荷进行加密，与数据包编号和头部偏移绑定。这在 DTLS 的粗粒度记录层中是不可能的。

+   **原生传输功能**：QUIC 实现了多路复用、独立流、每流流量控制和健壮的重传逻辑。DTLS 将可靠性和排序视为应用程序的责任。

+   **连接迁移能力**：QUIC 使用与 IP/端口端点解耦的连接 ID，使得网络接口切换更加平滑。DTLS 缺乏这种架构特性。

总结来说，QUIC 使用 TLS 1.3 进行加密握手，但**完全摒弃**了 DTLS，用紧密集成的传输协议来替代。这种设计使 QUIC 能够提供安全、低延迟、多路复用和移动友好的连接，而 DTLS（针对安全数据报通道优化的）无法与之匹敌。

## 介绍 quic-go

quic-go 实现了核心 IETF QUIC 规范，并支持生产使用所需的大多数功能，包括 TLS 1.3 集成、0-RTT、流多路复用和流量控制。虽然一些高级功能（如主动连接迁移）尚未实现。

### 开始使用 quic-go

要开始使用 `quic-go`，请通过 Go 模块包含它：

```go
`go  get  github.com/quic-go/quic-go` 
```

### 基本 QUIC 服务器

在 Go 中设置基本 QUIC 服务器与使用 `net` 包编写传统 TCP 服务器在概念上相似，但有几个重要的区别。

初始化阶段仍然涉及监听地址，但使用 `quic.ListenAddr()` 而不是 `net.Listen()`。与 TCP 不同，QUIC 在 UDP 上运行，并且从一开始就需要 TLS 配置，因为所有 QUIC 连接都是加密的。无需手动将连接包装在 TLS 中——QUIC 将加密作为协议的一部分来处理。

```go
 `listener,  err  :=  quic.ListenAddr("localhost:4242",  generateTLSConfig(),  nil)   if  err  !=  nil  {   log.Fatal(err)   }   fmt.Println("QUIC server listening on localhost:4242")    for  {   conn,  err  :=  listener.Accept(context.Background())   if  err  !=  nil  {   log.Println("Accept error:",  err)   continue   }   go  handleConn(conn)   }` 
```

接受连接后，处理与传统 `net.Conn` 模型差异更大。单个 QUIC 连接支持多个独立的流，每个流都像一个轻量级、有序、双向的字节流。这些流独立接受和处理，允许在单个连接上并发交互，而不存在头阻塞。

```go
 `defer  conn.CloseWithError(0,  "bye")    for  {   stream,  err  :=  conn.AcceptStream(context.Background())   if  err  !=  nil  {   return   }    go  func(s  quic.Stream)  {   defer  s.Close()    data,  err  :=  io.ReadAll(s)   if  len(data)  >  0  {   log.Printf("Received: %s",  string(data))   }   if  err  !=  nil  &&  err  !=  io.EOF  {   if  appErr,  ok  :=  err.(*quic.ApplicationError);  !ok  ||  appErr.ErrorCode  !=  0  {   log.Println("read error:",  err)   }   }   }(stream)   }` 
```

这种初始化和每流处理的分离是 QUIC 最强大的功能之一。在 TCP 中，一个连接等于一个流。在 QUIC 中，一个连接可以携带数十个并发、完全独立的流，每个流都有独立的流量控制和恢复行为，允许以最小延迟进行高效通信。

## 多路复用流

QUIC 内置支持流多路复用，允许在不增加额外连接开销的情况下进行双向通信。流独立操作，防止头阻塞，从而提高吞吐量和降低延迟。

```go
`stream,  err  :=  session.OpenStreamSync(context.Background()) if  err  !=  nil  {   log.Fatal(err) } _,  err  =  stream.Write([]byte("Hello QUIC!"))` 
```

## 性能：QUIC 与 HTTP/2 和 TCP 的比较

在性能基准测试中，QUIC 通常优于传统的基于 TCP 的 HTTP/2，尤其是在移动环境中常见的有损网络上。由于多路复用流和内置的拥塞控制算法（如 Cubic 和 BBR），这些算法直接集成到 quic-go 库中，QUIC 从数据包丢失中恢复得更快。

## 连接迁移

QUIC 协议的一个显著优势是其对无缝连接迁移（1）的支持，旨在允许移动设备在网络切换（例如，从 Wi-Fi 切换到蜂窝网络）时保持连接。这是通过连接 ID 实现的，它抽象化了客户端的 IP 地址和端口，即使客户端的网络路径发生变化，服务器也能继续通信。

1.  请参阅 [RFC 9000](https://datatracker.ietf.org/doc/html/rfc9000) 中的 [9.2\. 初始化连接迁移](https://datatracker.ietf.org/doc/html/rfc9000?section-9.2#name-initiating-connection-migra) 和 [9.3\. 响应连接迁移](https://datatracker.ietf.org/doc/html/rfc9000?section-9.2#name-responding-to-connection-mi) 部分。

然而，在实际应用中，连接迁移取决于特定的实现。在 quic-go（截至 v0.52.0 版本）中，尚未实现完全主动迁移。以下是支持的功能：

+   **NAT 重新绑定有效：** 如果客户端的 IP 或端口因 NAT 行为或 DHCP 更新而更改，并且使用相同的连接 ID，quic-go 将继续会话而无需建立新的连接。这是一种被动迁移，不需要客户端采取任何显式操作。

+   **接口切换（主动迁移）不受支持：** 切换网络接口（例如，从 Wi-Fi 切换到 LTE）需要通过新的路径发送数据包，并使用 PATH_CHALLENGE 和 PATH_RESPONSE 帧进行验证。协议定义了这种行为，但 quic-go 没有实现它。

## 实际应用的弹性

QUIC 在网络可靠性难以保证的情况下特别有效——例如在 LTE 上的移动客户端、在损耗链路上的物联网节点，或任何在接入点之间移动的边缘系统。QUIC 不是依赖于像前向纠错（在 IETF 标准化过程中被丢弃）这样的技术，而是通过流隔离、精细恢复和灵活路由的组合将鲁棒性构建到核心传输中。

QUIC 连接是复用的：每个流独立运行，具有单独的流量控制和交付状态。一个流上的数据包丢失不会干扰其他流，避免了 TCP 中固有的头阻塞问题。仅此一点就使 QUIC 在链路性能下降时在吞吐量上具有明显的优势。

QUIC 中的损失恢复由数据包级确认和基于 RTT 的计时器驱动——不依赖于 TCP 样式的重传逻辑。丢失的数据包检测得更快，重传的范围限于涉及的特定帧。

```go
sequenceDiagram
    participant Client
    participant Server

    Note over Client, Server: RTT = 100ms

    Client->>Server: Packet #1 (STREAM)
    Client->>Server: Packet #2 (STREAM)
    Client--xServer: Packet #3 (lost)
    Client->>Server: Packet #4 (STREAM)

    Note over Server: Server receives #1, #2, #4 (but not #3)

    Server-->>Client: ACK [1-2, 4] after 100ms

    Note over Client: Missing #3 is inferred lost\nonce #4 is ACKed before it

    Client->>Server: Packet #5 (Retransmit lost frame from #3)
    Server-->>Client: ACK [5]
```

### QUIC 的 ACK 与 TCP 有何不同

虽然 TCP 包括对 SACK（选择性确认）的支持，但有一些关键的区别使得 QUIC 的损失恢复更加鲁棒和确定：

+   **TCP 中的 SACK 是可选的**，并在握手过程中协商。QUIC 的 ACK 范围始终启用，并作为核心协议的一部分。

+   **TCP 确认字节**，而 QUIC 通过编号确认整个数据包。

+   **TCP 中的重传**通常是字节特定的，并与流位置相关联。在 QUIC 中，整个数据包都会被重传。

+   **QUIC 的 ACK 是加密的**，这使得它们具有抗篡改性和对路径干扰的鲁棒性。

| 功能 | TCP | QUIC |
| --- | --- | --- |
| ACK 粒度 | 字节级 | 数据包级 |
| ACK 范围支持 | 可选（通过 SACK） | 强制 |
| 损失检测 | 重复 ACK、计时器 | 间隔 + 基于 RTT 的计时器 |
| 重传粒度 | 部分流字节 | 整个数据包 |
| ACK 加密 | 否 | 是 |

### 连接迁移和中间盒鲁棒性

当设备在多个网络之间移动时（例如，从 Wi-Fi 切换到 LTE），QUIC 使用连接 ID 的功能使其能够保持连接的连续性。连接不绑定到 IP 端口对，也不需要完全重新连接。尽管`quic-go`目前还不支持主动迁移，但被动重绑定在实践中已经可行。

由于 QUIC 加密了其传输元数据，它对中间盒干扰也更具鲁棒性。加密的数据包编号、ACK 和控制帧减少了路径设备意外行为的风险，这可能会降低 TCP 的性能。

### 压缩控制灵活性

最后，QUIC 允许可插拔的拥塞控制。该协议不指定一个算法——BBR、Cubic 和自定义逻辑在应用层都是可能的。这允许针对不同的延迟和吞吐量权衡进行精细调整。

## 0-RTT 连接

QUIC 支持 0-RTT 握手，允许客户端在重复连接的初始握手期间发送应用程序数据。这显著降低了启动延迟。然而，由于 0-RTT 数据可能被攻击者重放，因此必须谨慎使用——通常限于幂等操作和受信任的客户端。

1.  重放攻击发生在攻击者捕获有效的网络数据（如请求或握手）并恶意重新传输它以欺骗服务器再次执行它的情况下。在 0-RTT 的上下文中，由于早期数据在握手完成之前发送，它可能被不同连接上的对手重放，可能导致重复操作（如重复购买或未经授权的状态更改）。这就是为什么 0-RTT 数据必须是幂等的或明确地防止重放的原因。

```go
 `earlyConn,  err  :=  quic.DialAddrEarly(context.Background(),  "localhost:4242",  tlsConf,  nil)` 
```

0-RTT 对于游戏、VoIP 和实时金融数据流等对延迟敏感的应用程序特别有益。

<details class="example"><summary>显示完整的 0-RTT 服务器/客户端示例</summary> <details class="example"><summary>0-RTT 服务器</summary>

```go
`package  main  import  (   "context"   "crypto/tls"   "log"   "time"    "github.com/quic-go/quic-go" )  func  main()  {   sessionCache  :=  tls.NewLRUClientSessionCache(128)   tlsConf  :=  &tls.Config{   InsecureSkipVerify:  true,   EnableActiveMigration:  true,   ClientSessionCache:  sessionCache,   NextProtos:  []string{"quic-0rtt-example"},   }    // 1\. Establish initial connection for session priming   conn,  err  :=  quic.DialAddr(context.Background(),  "localhost:4242",  tlsConf,  nil)   if  err  !=  nil  {   log.Fatal(err)   }   conn.CloseWithError(0,  "primed")   time.Sleep(500  *  time.Millisecond)    // 2\. 0-RTT connection   // DialAddrEarly-start   earlyConn,  err  :=  quic.DialAddrEarly(context.Background(),  "localhost:4242",  tlsConf,  nil)   // DialAddrEarly-end   if  err  !=  nil  {   log.Fatal("0-RTT failed:",  err)   }   stream,  err  :=  earlyConn.OpenStreamSync(context.Background())   if  err  !=  nil  {   log.Fatal("stream open failed:",  err)   }    message  :=  "Hello over 0-RTT"   if  _,  err  :=  stream.Write([]byte(message));  err  !=  nil  {   log.Fatal("write failed:",  err)   }   // Wait briefly to ensure data is sent before closing   time.Sleep(200  *  time.Millisecond)    stream.Close()   earlyConn.CloseWithError(0,  "done")   log.Println("0-RTT client sent:",  message) }` 
```</details> <details class="example"><summary>0-RTT 客户端</summary>

```go
`package  main  import  (   "context"   "crypto/tls"   "log"   "time"    "github.com/quic-go/quic-go" )  func  main()  {   sessionCache  :=  tls.NewLRUClientSessionCache(128)   tlsConf  :=  &tls.Config{   InsecureSkipVerify:  true,   EnableActiveMigration:  true,   ClientSessionCache:  sessionCache,   NextProtos:  []string{"quic-0rtt-example"},   }    // 1\. Establish initial connection for session priming   conn,  err  :=  quic.DialAddr(context.Background(),  "localhost:4242",  tlsConf,  nil)   if  err  !=  nil  {   log.Fatal(err)   }   conn.CloseWithError(0,  "primed")   time.Sleep(500  *  time.Millisecond)    // 2\. 0-RTT connection   // DialAddrEarly-start   earlyConn,  err  :=  quic.DialAddrEarly(context.Background(),  "localhost:4242",  tlsConf,  nil)   // DialAddrEarly-end   if  err  !=  nil  {   log.Fatal("0-RTT failed:",  err)   }   stream,  err  :=  earlyConn.OpenStreamSync(context.Background())   if  err  !=  nil  {   log.Fatal("stream open failed:",  err)   }    message  :=  "Hello over 0-RTT"   if  _,  err  :=  stream.Write([]byte(message));  err  !=  nil  {   log.Fatal("write failed:",  err)   }   // Wait briefly to ensure data is sent before closing   time.Sleep(200  *  time.Millisecond)    stream.Close()   earlyConn.CloseWithError(0,  "done")   log.Println("0-RTT client sent:",  message) }` 
```</details>

**预期输出**

当服务器启动并且客户端随后立即执行时，你应该看到：

***服务器控制台***

```go
`QUIC server listening on localhost:4242 2025/06/23 16:33:00 Received: Hello over 0-RTT` 
```

***客户端控制台***

```go
`0-RTT client sent: Hello over 0-RTT` 
```</details>

这证实了在恢复会话的 0-RTT 阶段，数据已经传输并被接受，无需等待完整握手完成。

## 关于 QUIC 的最终思考

QUIC 是一种具有显著设计优势的变革性协议，与 TCP 和 HTTP/2 相比，尤其是在以移动优先和实时系统为背景的情况下。它能够无阻塞地多路复用流，通过 0-RTT 减少握手延迟，并优雅地从数据包丢失中恢复，这使得它在具有不稳定连接性的环境中特别有效——例如 LTE、Wi-Fi 漫游或卫星上行链路。

虽然 Go 生态系统从成熟的用户空间实现 quic-go 中受益，但了解当前的局限性很重要。最值得注意的是，quic-go 尚未支持在[RFC 9000](https://datatracker.ietf.org/doc/html/rfc9000)中描述的完整主动连接迁移。尽管它被动地处理 NAT 重新绑定——在相同网络内的地址变化中保持会话——但它缺乏全多宿主或漫游支持所需的路径验证和接口切换逻辑。

同时，Linux 内核的 QUIC 实现正在积极开发中，旨在在 TCP 和 UDP 协议之外提供对协议的原生支持。这项由 Lucien Xin [led by Lucien Xin](https://lwn.net/ml/all/cover.1725935420.git.lucien.xin@gmail.com/) 领导的工作，在内核中提出了完整的 QUIC 堆栈，包括 kTLS 集成和基于套接字的 API 兼容性。如果被采用，这将解锁在高负载下 QUIC 的新性能上限，绕过用户空间复制开销，并减少数据平面操作的系统调用成本。

简而言之，QUIC 的架构非常适合超越传统的传输协议——尤其是在网络条件多变的情况下。虽然 quic-go 已经实现了许多这些优势，但值得记住的是，今天已实现的功能与规范中定义的功能之间的差异。随着生态系统支持的加深——从内核集成到高级路径管理——QUIC 的全部潜力将更容易为在延迟、可靠性和移动性边缘运行的系统所利用。

通过 quic-go 库使用 QUIC 为开发者提供了适用于现代网络需求的传输层。其内置的流多路复用、0-RTT 快速连接设置以及处理网络路径变化的能力，使其非常适合对延迟和可靠性至关重要的实时系统和移动应用。
