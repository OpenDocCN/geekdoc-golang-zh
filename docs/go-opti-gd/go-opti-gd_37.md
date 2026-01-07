# 在Go中比较TCP、HTTP/2和gRPC性能

> [https://goperf.dev/02-networking/tcp-http2-grpc/](https://goperf.dev/02-networking/tcp-http2-grpc/)

在分布式系统中，通信协议的选择决定了服务在现实负载下的交互方式。它不仅影响原始吞吐量和延迟，还影响系统的扩展性、CPU和内存消耗量，以及系统在压力下的行为预测性。在本文中，我们剖析了三种突出的选项——原始TCP与自定义帧结构、通过Go的内置`net/http`包实现的HTTP/2以及gRPC——并通过详细的基准测试和实际场景来探索它们的性能特征。

## 原始TCP与自定义帧结构

原始TCP提供了最大灵活性，几乎没有任何协议开销，但这需要付出代价：所有消息边界、帧逻辑和错误处理都必须手动实现。由于TCP交付的是一个连续的字节流，没有固有的消息概念，应用程序必须明确定义如何分离和解释这些字节。

### 自定义帧协议

在TCP上处理消息边界的一种常见方式是使用长度前缀帧：每个消息都以一个4字节的头部开始，告诉接收者下一个要读取多少字节。长度以大端格式编码，遵循标准网络字节顺序，因此在不同系统之间表现一致。这种设置解决了TCP的一个核心问题——虽然它保证了可靠交付，但它不保留消息边界。如果事先不知道大小，接收者就没有办法知道一个消息在哪里结束，下一个消息在哪里开始。

TCP保证了字节可靠、有序的交付，但它不保留或指示消息边界。例如，如果客户端发送三个逻辑消息：

```go
[msg1][msg2][msg3] 
```

服务器可能会将它们作为一个连续的字节流接收，具有任意分段，例如：

```go
[msg1_part][msg2][msg3_part] 
```

TCP交付的是一个没有内置概念的消息开始和结束的连续字节流。这意味着接收者不能依赖于读取边界来推断消息边界——到达的可能是一个部分消息、多个消息连接在一起，或者任意两者的片段。为了在这样一个流上理解结构化数据，应用程序需要一个帧策略。长度前缀通过在前面包含消息的大小来实现这一点，这样接收者就知道在开始解析有效载荷之前应该期待多少字节。

#### 协议结构

虽然长度前缀是常见且高效的帧定界策略，但根据用例的不同，还有其他选项。存在其他帧定界策略，每种策略在简单性、健壮性和灵活性方面都有其自身的权衡。基于分隔符的帧定界使用特定的字节或序列（如 `\n` 或 `\0`）来表示消息的结束。它易于实现，但如果分隔符可以出现在有效载荷中，则可能很脆弱。固定大小帧通过使每个消息具有相同的长度来避免歧义，从而简化了解析和内存分配，但当消息大小变化时则不太适用。像 Protobuf 或 ASN.1 这样的自描述格式将长度和类型信息嵌入到有效载荷本身中，允许结构更丰富且随着时间的推移而演变，但需要在两端都具备更复杂的解析逻辑和模式意识。选择正确的方法取决于你需要多少控制，你的数据有多可预测，以及你愿意吸收多少复杂性。

长度前缀实现中的每个帧由以下部分组成：

```go
| Length (4 bytes) | Payload (Length bytes) | 
```

+   **长度：** 以大端格式（网络字节序）编码的4字节无符号整数，表示有效载荷中的字节数。

+   **有效载荷：** 长度任意的原始二进制数据。

使用 `binary.BigEndian.PutUint32` 确保帧长度以标准格式编码——最高有效字节优先。这与互联网协议标准（1）一致，允许进行可预测的解码和异构系统之间的可靠互操作。

1.  遵循定义在 [RFC 791，第3.1节](https://datatracker.ietf.org/doc/html/rfc791#section-3.1) 并在TCP（[RFC 793](https://datatracker.ietf.org/doc/html/rfc793)）等传输和应用协议中一致使用的网络字节序惯例。

```go
func writeFrame(conn net.Conn, payload []byte) error {
    frameLen := uint32(len(payload))
    buf := make([]byte, 4+len(payload))
    binary.BigEndian.PutUint32(buf[:4], frameLen)
    copy(buf[4:], payload)
    _, err := conn.Write(buf)
    return err
}

func readFrame(conn net.Conn) ([]byte, error) {
    lenBuf := make([]byte, 4)
    if _, err := io.ReadFull(conn, lenBuf); err != nil {
        return nil, err
    }
    frameLen := binary.BigEndian.Uint32(lenBuf)
    payload := make([]byte, frameLen)
    if _, err := io.ReadFull(conn, payload); err != nil {
        return nil, err
    }
    return payload, nil
} 
```

这种方法简单、高效且可预测，但它不提供内置的并发管理、请求多路复用或流量控制——这些必须由开发者显式管理。

#### 不利因素

虽然该协议高效且最小化，但它缺少了更复杂传输协议中常见的几个功能。原始TCP中缺少内置的帧定界功能意味着关键责任完全转移到应用层。没有支持多路复用，所以每个连接只能有一个逻辑消息在传输，除非手动建立额外的协调——这促使客户端打开多个连接以实现并行性。流量控制也不存在；与HTTP/2或gRPC不同，没有信号反馈压力的方法，这使得快速发送者很容易压倒慢速接收者，可能耗尽内存或引发崩溃。除非你自己将它们嵌入到有效载荷格式中，否则没有空间用于结构化元数据，如消息类型、压缩标志或跟踪上下文。错误处理也是临时的——没有协议级别的机制来传达故障，因此格式错误的帧或长度不正确的帧通常会导致突然的连接重置或不一致的状态。

在范围紧密、性能高的系统中，这些限制可能是可管理的，因为连接的两端都完全受控，且对协议行为有很好的理解。在这样的环境中，最小化开销和直接访问网络线可以证明权衡是合理的。但在更广泛的生产环境中——特别是涉及多个团队、不断变化的需求或不可信客户端的情况下——它们会引入重大风险。如果没有严格的验证、清晰的框架和健壮的错误处理，即使是微小的不一致也可能导致无声的损坏、资源泄露或难以诊断的故障。基于原始TCP需要精确的工程和长期维护纪律。

### 性能洞察

+   **延迟:** 由于最小化开销，可达到最低可实现的延迟；非常适合对延迟敏感的场景，如金融交易系统。

+   **吞吐量:** 极佳，仅受网络和应用层处理限制。

+   **CPU/内存成本:** 非常低，协议管理的开销可以忽略不计。

## 通过net/http使用HTTP/2

相比 HTTP/1.1，HTTP/2 在协议层面带来了多项改进，包括单个连接上的复用流、通过 HPACK 进行头部压缩以及支持服务器推送。在 Go 中，这些功能直接集成到 `net/http` 标准库中，该库处理连接重用、流复用和并发，无需手动干预。与必须显式定义消息边界的原始 TCP 不同，HTTP/2 在协议层面定义了这些边界：每个请求和响应都使用结构化的 `HEADERS` 和 `DATA` 帧进行封装，并使用 `END_STREAM` 标志显式关闭。这些帧完全由 Go 的 HTTP/2 实现处理，因此开发者可以使用标准的 `http.Request` 和 `http.ResponseWriter` 接口与完整、逻辑上隔离的消息交互。你无需处理字节流或担心消息的起始或结束位置——当请求到达你的处理器时，它已经被封装并解析。当你编写响应时，运行时会负责将其封装并发出完成信号。这让你可以专注于逻辑，而不是管道，同时仍然获得 HTTP/2 的性能优势，如复用和连接重用。

### 服务器实现

除了框架和复用之外，HTTP/2 还带来了一系列实用的优势，这些优势使得服务器代码更容易编写和运行更快。它默认处理连接重用，应用流量控制以避免任何一方过载，并使用 `HPACK` 压缩头部以减少开销。Go 的 `net/http` 栈在幕后处理所有这些，因此你无需自己配置即可获得这些好处。结果，开发者可以构建并发、高效的服务器，而无需手动管理低级别的连接或流状态。

```go
func handler(w http.ResponseWriter, r *http.Request) {
    payload, err := io.ReadAll(r.Body)
    if err != nil {
        http.Error(w, "invalid request", http.StatusBadRequest)
        return
    }
    defer r.Body.Close()

    // Process payload...

    w.WriteHeader(http.StatusOK)
    w.Write([]byte("processed"))
}

func main() {
    server := &http.Server{
        Addr:    ":8080",
        Handler: http.HandlerFunc(handler),
    }
    log.Fatal(server.ListenAndServeTLS("server.crt", "server.key"))
} 
```

信息

即使没有明确提及，此代码也支持 HTTP/2，因为它使用了 `ListenAndServeTLS`，这启用了基于 TLS 的通信。Go 的 `net/http` 包在 TLS 握手期间，如果客户端通过 ALPN（应用层协议协商）支持 HTTP/2，则会自动将连接升级到 HTTP/2。自 Go 1.6 以来，这种升级是隐式的——无需额外配置。服务器透明地处理 HTTP/2 请求，同时保持与 HTTP/1.1 客户端的兼容性。

HTTP/2 的复用能力允许多个独立的流共享单个 TCP 连接而不相互阻塞，这显著提高了连接重用率。这减少了建立和管理并行连接的开销，尤其是在高并发情况下。因此，延迟更低，吞吐量更稳定，即使有多个请求正在传输也是如此。这些特性使 HTTP/2 非常适合通用型网络服务和内部 API——在这些地方，可预测的延迟、高效的连接重用和良好的并发处理比原始协议的简约性更重要。

### 性能洞察

+   **延迟：** 由于帧和压缩开销，略高于原始TCP，但得益于多路复用和持久连接，稳定且一致。

+   **吞吐量：** 在并发负载下吞吐量高；流多路复用和头部压缩有助于维持性能，而无需打开更多套接字。

+   **CPU/内存开销：** 由于头部处理、TLS加密和流量控制机制，开销适中。

## gRPC

gRPC是一个基于HTTP/2构建的高性能、契约优先的RPC框架，旨在实现服务之间低延迟、跨语言的通信。它结合了具有流能力的传输和通过协议缓冲区（Protobuf）定义的强类型API，实现了紧凑、高效的消息序列化以及跨平台的无缝互操作性。与传统的HTTP API不同，后者通过URL模式和自由形式的JSON松散定义端点，gRPC通过`.proto`定义强制执行严格的接口契约，这些定义既作为模式也作为实现规范。gRPC工具链为多种语言生成客户端和服务器代码，消除了手动序列化，提高了安全性，并在异构系统之间标准化了交互。

gRPC利用HTTP/2的核心特性——流多路复用、流量控制和二进制帧——来支持一次性RPC调用和全双工流，所有这些都有内置的背压。但它不仅仅局限于传输。它内置了对截止日期、取消、结构化元数据和标准化错误报告的支持，所有这些都有助于服务清晰沟通并可预测地失败。这使得gRPC成为内部API、服务网格和需要效率、强契约和负载下的可靠行为的性能关键系统的可靠选择。

### gRPC服务定义

最小化的`.proto`文件示例：

```go
syntax = "proto3";

service EchoService {
  rpc Echo(EchoRequest) returns (EchoResponse);
}

message EchoRequest {
  string message = 1;
}

message EchoResponse {
  string message = 1;
} 
```

生成的Go存根允许开发者轻松实现服务：

```go
type server struct {
    UnimplementedEchoServiceServer
}

func (s *server) Echo(ctx context.Context, req *EchoRequest) (*EchoResponse, error) {
    return &EchoResponse{Message: req.Message}, nil
}

func main() {
    lis, err := net.Listen("tcp", ":50051")
    if err != nil {
        log.Fatalf("failed to listen: %v", err)
    }
    grpcServer := grpc.NewServer()
    RegisterEchoServiceServer(grpcServer, &server{})
    grpcServer.Serve(lis)
} 
```

### 性能洞察

+   **延迟：** 由于额外的序列化和反序列化步骤，略高于原始HTTP/2，但在大多数场景下仍然表现良好。

+   **吞吐量：** 由于有效的有效载荷序列化（protobuf）和内建的HTTP/2多路复用能力，吞吐量高。

+   **CPU/内存开销：** 由于protobuf编码开销，高于HTTP/2；由于临时对象分配，内存消耗略有增加。

## 选择正确的协议

+   **内部API和微服务：** 一旦工具就绪，gRPC通常能击中最佳平衡点——它快速、强类型，易于使用。

+   **低延迟系统和交易平台：** 使用原始TCP和自定义帧提供最低的开销，但其他所有事情都需要你自己处理。

+   **公共API或通用网络服务：** 通过net/http使用HTTP/2是一个可靠的选择。你将获得连接重用、多路复用和良好的性能，而无需引入完整的RPC堆栈。

原始TCP提供了最大的控制和纸面上的最佳性能——但这同时也意味着你需要自己构建一切：帧定界、流量控制、错误处理。HTTP/2和gRPC则牺牲了一些原始速度，以换取内置的结构、更好的连接处理和更少的维护代码。正确的选择完全取决于性能的重要性以及你愿意承担的复杂性。
