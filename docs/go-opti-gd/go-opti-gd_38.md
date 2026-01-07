# Go语言中的QUIC：使用quic-go构建低延迟服务

> 原文：[https://goperf.dev/02-networking/quic-in-go/](https://goperf.dev/02-networking/quic-in-go/)

QUIC已成为一种稳健的协议，解决了传统TCP连接的许多固有局限性。QUIC将加密、多路复用和连接迁移结合成一个统一的协议，旨在优化网络性能，尤其是在实时和以移动优先的应用程序中。在Go语言中，[quic-go](https://github.com/quic-go/quic-go)是主要的QUIC实现，并作为构建具有内置加密和流多路复用的有效、低延迟网络服务的实用基础。

## 理解QUIC

原先由谷歌开发，后来由IETF标准化，QUIC重新思考了传输层，以克服长期存在的TCP限制：

+   **头阻塞**：TCP交付一个有序的字节流，因此数据包丢失会阻止其后的所有内容。QUIC将数据分成独立的流，即使其中一个延迟，其他流也可以继续进行。

+   **按包加密和头部保护**：QUIC在数据包级别应用加密，包括与数据包编号相关的选择性头部保护——这是DTLS基于记录的帧格式所无法支持的。

+   **内置传输机制**：QUIC作为协议的一部分处理流多路复用、流量控制和重传。相比之下，DTLS仅对数据报进行加密，并将可靠性和排序留给应用程序。

+   **连接ID抽象**：QUIC使用连接ID而不是IP和端口号元组来标识会话，这使得连接能够在网络变化中持续存在。DTLS没有提供这种抽象，使得移动性难以实现。

### QUIC与TCP：主要区别

QUIC与TCP采取了根本不同的方法。虽然TCP直接建立在IP之上，并且数据可以流动之前需要建立面向连接的手势，但QUIC在UDP之上运行并处理自己的连接逻辑，减少了设置开销并提高了启动延迟。这种架构选择使得QUIC能够提供多路复用、独立的流，有效地消除了TCP中常见的头阻塞问题，即一个数据包的延迟或丢失会阻止后续数据包。

QUIC直接将其传输层集成到TLS 1.3中，消除了TCP+TLS中看到的分层协商。这种设计简化了握手过程，并实现了0-RTT数据，即重复连接可以立即开始传输加密的有效载荷——这是TCP所不支持的功能。

另一个关键的区别是连接的识别方式。TCP连接绑定到特定的IP和端口，因此任何网络接口的变化都会导致连接中断。QUIC通过使用在地址变化中保持稳定的连接ID来避免这种情况，允许设备在网络之间移动时无缝地继续会话——这对于移动和延迟敏感的使用案例至关重要。

## QUIC是基于DTLS的吗?

虽然QUIC和DTLS都使用TLS加密原语在UDP上操作，但QUIC**并不**基于DTLS。相反，QUIC直接将其传输层与TLS 1.3结合，仅继承加密握手——而不是DTLS的记录帧或协议结构。

QUIC定义了自己的数据包编码、多路复用、重传和加密格式。它将TLS握手消息封装在QUIC数据包中，并将加密状态与传输功能（如包号和流ID）紧密耦合。相比之下，DTLS作为UDP之上的安全数据报层运行，提供加密和认证，但将传输语义（如重传、排序或流量控制）留给高层。

QUIC拒绝DTLS作为其安全基础的原因包括：

+   **握手和传输的更紧密集成**：QUIC将TLS协商与传输状态设置合并，使0-RTT重用和1-RTT设置在更少的往返中完成。DTLS的分层模型引入了更高的延迟。

+   **细粒度加密控制**：QUIC按包对数据包头和有效载荷进行加密，绑定到包号和头偏移。这在DTLS的粗粒度记录层中是不可能的。

+   **原生传输特性**：QUIC实现了多路复用、独立流、每流流量控制和弹性重传逻辑。DTLS将可靠性和排序视为应用程序的责任。

+   **连接迁移能力**：QUIC使用与IP/端口端点解耦的连接ID，允许平滑的网络接口切换。DTLS缺乏这种架构特性。

总结来说，QUIC使用TLS 1.3进行加密握手，但**完全放弃了DTLS**，用紧密集成的传输协议来替代。这种设计使QUIC能够提供安全、低延迟、多路复用和移动友好的连接，这是DTLS（针对安全数据报通道优化）所无法比拟的。

## 介绍quic-go

quic-go实现了核心的IETF QUIC规范，并支持生产所需的大多数功能，包括TLS 1.3集成、0-RTT、流多路复用和流量控制。虽然一些高级功能（如主动连接迁移）尚未实现。

### 开始使用quic-go

要开始使用`quic-go`，通过Go模块包含它：

```go
go get github.com/quic-go/quic-go 
```

### 基本QUIC服务器

在 Go 中设置基本的 QUIC 服务器与使用 `net` 包编写传统的 TCP 服务器在概念上相似，但有几个重要的区别。

初始化阶段仍然涉及监听地址，但使用 `quic.ListenAddr()` 而不是 `net.Listen()`。与 TCP 不同，QUIC 在 UDP 上运行，并从一开始就需要 TLS 配置，因为所有 QUIC 连接都是设计上加密的。无需手动将连接包装在 TLS 中——QUIC 将加密作为协议的一部分处理。

```go
 listener, err := quic.ListenAddr("localhost:4242", generateTLSConfig(), nil)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("QUIC server listening on localhost:4242")

    for {
        conn, err := listener.Accept(context.Background())
        if err != nil {
            log.Println("Accept error:", err)
            continue
        }
        go handleConn(conn)
    } 
```

接受连接后，处理方式与传统 `net.Conn` 模型差异显著。单个 QUIC 连接支持多个独立的流，每个流都像一个轻量级、有序的双向字节流。这些流可以独立接受和处理，允许在单个连接上并发交互，而不会出现头阻塞。

```go
 defer conn.CloseWithError(0, "bye")

    for {
        stream, err := conn.AcceptStream(context.Background())
        if err != nil {
            return
        }

        go func(s quic.Stream) {
            defer s.Close()

            data, err := io.ReadAll(s)
            if len(data) > 0 {
                log.Printf("Received: %s", string(data))
            }
            if err != nil && err != io.EOF {
                if appErr, ok := err.(*quic.ApplicationError); !ok || appErr.ErrorCode != 0 {
                    log.Println("read error:", err)
                }
            }
        }(stream)
    } 
```

初始化和每个流处理之间的这种分离是 QUIC 最强大的功能之一。与 TCP 不同，TCP 中一个连接等于一个流。在 QUIC 中，一个连接可以携带数十个并发、完全独立的流，具有独立的流量控制和恢复行为，允许以最低的延迟进行高效通信。

## 多路复用流

QUIC 本身支持流多路复用，允许在不增加额外连接开销的情况下进行双向通信。流独立操作，防止头阻塞，从而提高吞吐量并降低延迟。

```go
stream, err := session.OpenStreamSync(context.Background())
if err != nil {
    log.Fatal(err)
}
_, err = stream.Write([]byte("Hello QUIC!")) 
```

## 性能：QUIC 与 HTTP/2 和 TCP 的比较

在性能基准测试中，QUIC 通常优于传统的基于 TCP 的 HTTP/2，尤其是在移动环境中常见的有损网络上。由于多路复用流和内置的拥塞控制算法（如 Cubic 和 BBR），这些算法直接集成到 quic-go 库中，QUIC 从数据包丢失中恢复得更快。

## 连接迁移

QUIC 协议的一个显著优势是其对无缝连接迁移（1）的支持，旨在允许移动设备在网络切换（例如，从 Wi-Fi 切换到蜂窝网络）时保持连接。这是通过连接 ID 实现的，它抽象化了客户端的 IP 地址和端口，使得即使在客户端的网络路径发生变化的情况下，服务器也能继续通信。

1.  请参阅 [RFC 9000](https://datatracker.ietf.org/doc/html/rfc9000) 中的 [9.2. 启动连接迁移](https://datatracker.ietf.org/doc/html/rfc9000?section-9.2#name-initiating-connection-migra) 和 [9.3. 响应连接迁移](https://datatracker.ietf.org/doc/html/rfc9000?section-9.2#name-responding-to-connection-mi) 部分。

然而，在实践中，连接迁移取决于具体的实现。在 quic-go（截至 v0.52.0 版本）中，尚未实现完全的主动迁移。以下是支持的功能：

+   **NAT重绑定工作**：如果客户端的IP或端口由于NAT行为或DHCP更新而更改，并且使用相同的连接ID，quic-go将无需新的连接继续会话。这是被动迁移，不需要客户端采取任何明确行动。

+   **接口切换（主动迁移）不受支持**：切换网络接口（例如，从Wi-Fi切换到LTE）需要从新路径发送数据包，并使用PATH_CHALLENGE和PATH_RESPONSE帧进行验证。该协议定义了这种行为，但quic-go没有实现它。

## 实践中的弹性

QUIC在难以保证可靠性的网络中特别有效——例如在LTE上的移动客户端、在损耗链路上的物联网节点，或任何在接入点之间移动的边缘系统。QUIC不是依赖于诸如前向纠错（在IETF标准化过程中被丢弃）等技术，而是通过流隔离、精细恢复和灵活路由的组合将弹性构建到核心传输中。

QUIC连接是复用的：每个流独立运行，具有独立的流量控制和交付状态。一个流上的数据包丢失不会干扰其他流，避免了TCP中固有的头阻塞问题。这本身就让QUIC在链路退化时的吞吐量上具有明显的优势。

QUIC中的损失恢复由数据包级别的确认和基于RTT的定时器驱动——不依赖于TCP风格的重新传输逻辑。丢失的数据包检测更快，重传的范围限定在特定的帧中。

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

### QUIC的ACK与TCP的不同

虽然TCP包括对SACK（选择性确认）的支持，但有一些关键的区别使得QUIC的损失恢复更加健壮和确定：

+   **TCP中的SACK是可选的**，并在握手过程中协商。QUIC的ACK范围始终启用，并作为核心协议的一部分。

+   **TCP确认字节**，而QUIC通过编号确认整个数据包。

+   **TCP中的重传**通常是字节特定的，并与流位置相关联。在QUIC中，整个数据包都会被重传。

+   **QUIC的ACK是加密的**，这使得它们具有抗篡改性和对路径干扰的鲁棒性。

| 功能 | TCP | QUIC |
| --- | --- | --- |
| ACK粒度 | 字节级 | 数据包级 |
| ACK范围支持 | 可选（通过SACK） | 强制 |
| 损失检测 | 重复确认，定时器 | 空闲时间 + 基于RTT的定时器 |
| 重传粒度 | 部分流字节 | 整个数据包 |
| ACK加密 | 否 | 是 |

### 连接迁移和中继盒弹性

当设备在网络上移动（例如，从Wi-Fi切换到LTE）时，QUIC使用连接ID的功能使其能够保持连续性。连接不绑定到IP端口对，也不需要完全重新连接。尽管`quic-go`尚未支持主动迁移，但被动重绑定在实践中已经可行。

因为QUIC加密了其传输元数据，它对中间盒干扰的抵抗力也更强。加密的数据包编号、ACK和控制帧减少了路径上设备意外行为的风险，这可能会降低TCP性能。

### 拥塞控制灵活性

最后，QUIC支持可插拔的拥塞控制。该协议不指定一个算法——BBR、Cubic和自定义逻辑在应用层都是可能的。这允许针对不同的延迟和吞吐量权衡进行精细调整。

## 0-RTT 连接

QUIC支持0-RTT握手，允许客户端在重复连接的初始握手期间发送应用程序数据。这显著减少了启动延迟。然而，由于0-RTT数据可能被攻击者重放，它必须谨慎使用——通常限于幂等操作和受信任的客户端。

1.  重放攻击发生在攻击者捕获有效的网络数据——例如请求或握手——并恶意重新传输它以欺骗服务器再次执行它的情况下。在0-RTT的上下文中，由于早期数据在握手完成之前发送，它可能被不同连接的对手重放，这可能导致重复操作（如双重购买或未经授权的状态更改）。这就是为什么0-RTT数据必须是幂等的或明确地防止重放的原因。

```go
 earlyConn, err := quic.DialAddrEarly(context.Background(), "localhost:4242", tlsConf, nil) 
```

0-RTT对于游戏、VoIP和实时金融数据流等对延迟敏感的应用程序特别有益。

<details class="example"><summary>显示完整的0-RTT服务器/客户端示例</summary> <details class="example"><summary>0-RTT 服务器</summary>

```go
package main

import (
    "context"
    "crypto/tls"
    "log"
    "time"

    "github.com/quic-go/quic-go"
)

func main() {
    sessionCache := tls.NewLRUClientSessionCache(128)
    tlsConf := &tls.Config{
        InsecureSkipVerify: true,
        EnableActiveMigration: true,
        ClientSessionCache: sessionCache,
        NextProtos:         []string{"quic-0rtt-example"},
    }

    // 1\. Establish initial connection for session priming
    conn, err := quic.DialAddr(context.Background(), "localhost:4242", tlsConf, nil)
    if err != nil {
        log.Fatal(err)
    }
    conn.CloseWithError(0, "primed")
    time.Sleep(500 * time.Millisecond)

    // 2\. 0-RTT connection
    // DialAddrEarly-start
    earlyConn, err := quic.DialAddrEarly(context.Background(), "localhost:4242", tlsConf, nil)
    // DialAddrEarly-end
    if err != nil {
        log.Fatal("0-RTT failed:", err)
    }
    stream, err := earlyConn.OpenStreamSync(context.Background())
    if err != nil {
        log.Fatal("stream open failed:", err)
    }

    message := "Hello over 0-RTT"
    if _, err := stream.Write([]byte(message)); err != nil {
        log.Fatal("write failed:", err)
    }
    // Wait briefly to ensure data is sent before closing
    time.Sleep(200 * time.Millisecond)

    stream.Close()
    earlyConn.CloseWithError(0, "done")
    log.Println("0-RTT client sent:", message)
} 
```</details> <details class="example"><summary>0-RTT 客户端</summary>

```go
package main

import (
    "context"
    "crypto/tls"
    "log"
    "time"

    "github.com/quic-go/quic-go"
)

func main() {
    sessionCache := tls.NewLRUClientSessionCache(128)
    tlsConf := &tls.Config{
        InsecureSkipVerify: true,
        EnableActiveMigration: true,
        ClientSessionCache: sessionCache,
        NextProtos:         []string{"quic-0rtt-example"},
    }

    // 1\. Establish initial connection for session priming
    conn, err := quic.DialAddr(context.Background(), "localhost:4242", tlsConf, nil)
    if err != nil {
        log.Fatal(err)
    }
    conn.CloseWithError(0, "primed")
    time.Sleep(500 * time.Millisecond)

    // 2\. 0-RTT connection
    // DialAddrEarly-start
    earlyConn, err := quic.DialAddrEarly(context.Background(), "localhost:4242", tlsConf, nil)
    // DialAddrEarly-end
    if err != nil {
        log.Fatal("0-RTT failed:", err)
    }
    stream, err := earlyConn.OpenStreamSync(context.Background())
    if err != nil {
        log.Fatal("stream open failed:", err)
    }

    message := "Hello over 0-RTT"
    if _, err := stream.Write([]byte(message)); err != nil {
        log.Fatal("write failed:", err)
    }
    // Wait briefly to ensure data is sent before closing
    time.Sleep(200 * time.Millisecond)

    stream.Close()
    earlyConn.CloseWithError(0, "done")
    log.Println("0-RTT client sent:", message)
} 
```</details>

**预期输出**

当服务器启动并且客户端随后立即执行时，你应该看到：

***服务器控制台***

```go
QUIC server listening on localhost:4242
2025/06/23 16:33:00 Received: Hello over 0-RTT 
```

***客户端控制台***

```go
0-RTT client sent: Hello over 0-RTT 
```</details>

这证实了在恢复会话的0-RTT阶段已传输并接受早期数据，而无需等待完整握手完成。

## 关于使用Go的QUIC的最终思考

QUIC是一个具有显著设计优势的变革性协议，与TCP和HTTP/2相比，特别是在以移动优先和实时系统为背景的情况下。它能够在没有头阻塞的情况下多路复用流，通过0-RTT减少握手延迟，并从数据包丢失中优雅地恢复，这使得它在具有不稳定连接性的环境中特别有效——如LTE、Wi-Fi漫游或卫星上行链路。

虽然 Go 生态系统受益于成熟的用户空间实现 quic-go，但了解当前的局限性很重要。最值得注意的是，quic-go 还不支持 [RFC 9000](https://datatracker.ietf.org/doc/html/rfc9000) 中描述的完整主动连接迁移。尽管它能够被动处理 NAT 重新绑定——在相同网络内地址变化时维持会话——但它缺乏实现完整多宿主或漫游支持所需的路径验证和接口切换逻辑。

同时，Linux 内核的 QUIC 实现正在积极开发中，旨在在 TCP 和 UDP 之外提供对协议的原生支持。这项由 [Lucien Xin](https://lwn.net/ml/all/cover.1725935420.git.lucien.xin@gmail.com/) 领导的工作，在内核内提出了完整的 QUIC 堆栈，包括 kTLS 集成和基于套接字的 API 兼容性。如果被采用，这将解锁在高负载下 QUIC 的新性能上限，绕过用户空间复制开销并降低数据平面操作的系统调用成本。

简而言之，QUIC 的架构非常适合超越传统的传输协议——特别是在可变网络条件下。虽然 quic-go 已经使许多这些好处成为可能，但值得记住今天已实现的内容与规范中定义的内容之间的差异。随着生态系统支持的加深——从内核集成到高级路径管理——QUIC 的全部潜力将更容易为在延迟、可靠性和移动性边缘运行的系统所利用。

通过 quic-go 库使用 QUIC 为开发者提供了专为现代网络需求设计的传输层。其内置的流多路复用、0-RTT 快速连接设置以及处理网络路径变化的能力，使其非常适合对延迟和可靠性至关重要的实时系统和移动应用。
