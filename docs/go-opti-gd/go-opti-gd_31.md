# 优化 TLS 以提高速度：握手、重用和加密套件选择

> 原文：[`goperf.dev/02-networking/tls-for-speed/`](https://goperf.dev/02-networking/tls-for-speed/)

TLS 做了它应该做的事情：它保护你的连接私密且值得信赖。但这也减慢了速度——比大多数人意识到的要多得多。在 Go 语言中，如果你关心你的服务响应速度，你可以通过调整 TLS 的协商方式和协商内容来提高性能。

## 理解 TLS 过度开销：性能受影响的区域

TLS 的大部分减速发生在开始阶段。握手是一个往返过程：选择算法、交换密钥、证明身份和设置会话。这种往返通常需要网络上的两次完整往返。在像交易平台或实时应用这样的场景中，这种延迟是明显的。

要使 TLS 更快，最有效的方法是减少握手步骤并使加密工作成本更低。

```go
sequenceDiagram
    participant Client
    participant Server

    Client->>Server: ClientHello (supported ciphers, random)
    Server->>Client: ServerHello (chosen cipher, random)
    Server->>Client: Certificate
    Server->>Client: ServerKeyExchange
    Client->>Server: ClientKeyExchange
    Client->>Server: ChangeCipherSpec
    Server->>Client: ChangeCipherSpec
    Note over Client,Server: Handshake Complete – Encrypted communication begins
```

## 会话恢复：减少握手延迟

因为每个新的 TLS 连接都会运行完整的握手——协商加密套件、交换密钥、验证证书——它引入了明显的延迟。会话恢复通过重用早期会话的加密状态来规避大部分延迟，使重新连接变得更快。

会话恢复是 TLS 中的一种机制，用于避免在重新连接时重复完整的握手过程。主要有两种方法：会话 ID 和会话票据。两者都依赖于服务器记住（或编码）从先前的连接中会话的加密状态。当客户端重新连接时，它会提供会话 ID 或会话票据，允许服务器恢复会话状态并跳过昂贵的非对称密钥交换。

会话票据是服务器在握手结束时向客户端发出的数据块。此票据包含加密的会话状态（例如协商的加密套件、密钥和会话参数），对客户端来说是透明的。在重新连接时，客户端将票据发送回服务器，服务器将其解密以在无需进行完整握手的情况下恢复会话。

在 Go 语言中，你可以通过设置会话票据密钥来启用会话恢复。服务器使用这些密钥来加密和解密客户端在恢复连接时发送回的会话状态。你可以在启动时使用 crypto/rand 生成一个安全的 32 字节密钥，如果你的服务在负载均衡器后面的多个实例上运行，则可以重用它。只需确保定期更换密钥以保持其安全性。

```go
`tlsConfig  :=  &tls.Config{   SessionTicketsDisabled:  false,  // Enable session tickets explicitly   SessionTicketKey:  [32]byte{...},  // Persist securely and rotate periodically }` 
```

会话恢复之所以有效，是因为它避免了重新执行 TLS 中最慢的部分。服务器不是从头开始协商一切，而是解密票据、验证它并恢复会话。这至少消除了一个完整的往返和所有重型的非对称操作，从而节省了时间和资源。

## 聪明地选择加密套件

加密套件定义了用于密钥交换、身份验证、加密和完整性检查的算法组合。选择高效的加密套件对性能有重大影响。

为什么选择一个而不是另一个？一些套件使用 RSA 进行密钥交换，这比椭圆曲线 Diffie-Hellman (ECDHE) 慢得多，并且缺乏前向保密。具有 AES-GCM 的套件在现代 CPU 上由于硬件加速而更快，而较旧的 CBC 模式套件较慢且更容易出错。基于 ECC 的身份验证（ECDSA）与 RSA 相比，提供更短的签名和更低的计算成本。

为了明智地选择：

+   优先选择具有 ECDHE 的套件以实现前向保密和更好的性能。

+   优先选择 AES-GCM 而不是 AES-CBC 或 ChaCha20，除非目标是缺乏 AES 加速功能的硬件（在这种情况下 ChaCha20 可能更胜一筹）。

+   避免仅使用 RSA 进行密钥交换的套件，因为它们缺乏前向保密，并且在服务器端需要更昂贵的计算。

Go 的标准库提供了一套稳健的安全和合理快速的默认设置。对于大多数应用程序，你可以坚持使用这些设置，并且效果良好。但如果你正在为高性能环境进行调优，定义自己的首选加密套件可以让你更紧密地控制要协商的内容。

一个优化的配置优先选择 AES-GCM 以利用其硬件加速，并选择 ECC 以提高效率。

```go
`tlsConfig  :=  &tls.Config{   CipherSuites:  []uint16{   tls.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,   tls.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,   tls.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,   tls.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,   },   PreferServerCipherSuites:  true, }` 
```

这些套件在安全性和速度之间取得了良好的平衡，利用了前向保密和硬件加速加密。第二个选项，TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256，当客户端可能不支持 ECDSA 证书时是一个合理的选择——它仍然提供了前向保密和高效的 AES-GCM 加密，同时依赖于更广泛部署的 RSA 进行身份验证。

## 使用 ALPN 的明智之处

应用层协议协商 (ALPN) 允许客户端和服务器在 TLS 握手期间就应用协议（如 HTTP/2、HTTP/1.1 或 gRPC）达成一致，避免了在建立连接后的额外往返或猜测。没有 ALPN，客户端将不得不回退到较慢或效率较低的方法来检测服务器支持协议。

在 Go 中，`tls.Config` 中的 `NextProtos` 字段定义了服务器支持的应用协议及其优先顺序。在 TLS 握手过程中，客户端发送自己的列表，服务器选择双方都支持的最高优先级协议，然后确认它。

如果你想要支持 `HTTP/2` 并回退到 `HTTP/1.1`，你应该显式设置 `NextProtos`，如下所示：

```go
`tlsConfig  :=  &tls.Config{   NextProtos:  []string{"h2",  "http/1.1"}, }` 
```

`NextProtos` 中条目的顺序很重要，因为服务器会按顺序遍历列表，并选择客户端也提供的第一个协议。通过将 HTTP/2 (`h2`) 放在顶部，服务器确保在支持的情况下优先选择 HTTP/2，只有在必要时才回退到 HTTP/1.1。如果顺序不正确，服务器可能会选择一个较慢的协议，或者完全无法达成一致。

当`NextProtos`留空时，ALPN 实际上被禁用。客户端假设 HTTP/1.1，因为它在握手过程中从未看到对 HTTP/2 支持的任何指示。显式设置`NextProtos`可以宣传服务器的能力，并避免不必要的协议降级。

## 最小化证书验证开销

证书验证通常很耗时，因为它涉及几个昂贵的非对称加密操作，这直接增加了连接延迟。这种成本可以通过两种方式降低：使用更小、验证速度更快的基于 ECC（ECDSA）证书而不是 RSA，以及通过缓存已验证的证书链以避免在后续连接中重复相同的验证工作。

```go
`tlsConfig  :=  &tls.Config{   ClientAuth:  tls.RequireAndVerifyClientCert,   ClientCAs:  certPool,  // pre-verified CA pool   VerifyPeerCertificate:  cachedCertVerifier,  // custom verifier with caching }` 
```

基本的缓存机制临时存储验证结果：

```go
`// Cache to avoid re-verifying the same certificate repeatedly var  verificationCache  sync.Map  // cachedCertVerifier verifies a peer certificate chain and caches successful leaf fingerprints func  cachedCertVerifier(rawCerts  [][]byte,  verifiedChains  [][]*x509.Certificate)  error  {   // Compute SHA-256 fingerprint of the leaf certificate   fingerprint  :=  sha256.Sum256(rawCerts[0])   if  _,  exists  :=  verificationCache.Load(fingerprint);  exists  {   // Already verified earlier; skip full verification   return  nil   }    // Parse the leaf certificate   leafCert,  err  :=  x509.ParseCertificate(rawCerts[0])   if  err  !=  nil  {   return  fmt.Errorf("failed to parse leaf certificate: %w",  err)   }    // Build pool of intermediate certificates provided by peer   intermediatePool  :=  x509.NewCertPool()   for  _,  raw  :=  range  rawCerts[1:]  {   intermediateCert,  err  :=  x509.ParseCertificate(raw)   if  err  !=  nil  {   return  fmt.Errorf("failed to parse intermediate certificate: %w",  err)   }   intermediatePool.AddCert(intermediateCert)   }    // Prepare verification options with trusted roots & intermediates   opts  :=  x509.VerifyOptions{   Roots:  certPool,  // trusted root CAs   Intermediates:  intermediatePool,  // peer-provided intermediates   KeyUsages:  []x509.ExtKeyUsage{x509.ExtKeyUsageServerAuth},   }    // Verify the full chain starting from leaf   chains,  err  :=  leafCert.Verify(opts)   if  err  ==  nil  {   // Store successful verification in cache   verificationCache.Store(fingerprint,  struct{}{})   }   return  err }` 
```

警告

虽然该片段说明了关键概念，但不应直接在生产环境中使用。加密代码需要仔细分析和适应特定环境和威胁模型。在部署前始终验证和测试与安全相关的代码，以避免引入弱点。

## Go 中的 TLS 最佳实践

以下配置将这些技术结合在一起，形成一个针对性能和安全都进行了优化的 tls.Config。

`MinVersion`设置告诉服务器拒绝较旧、有缺陷的 TLS 版本。低于 TLS 1.2 的任何内容——如 SSLv3、TLS 1.0 或 1.1——都存在许多弱点，不再被认为是安全的。TLS 1.2 通过引入 AEAD 加密（如 AES-GCM）和针对 BEAST 和 POODLE 等攻击的防御措施，解决了许多这些问题。

`CurvePreferences`字段优先考虑大多数客户端支持的快速且安全的椭圆曲线。P-256 和 X25519 效率高、支持广泛，并且通常在硬件上加速。P-256（也称为 secp256r1）是一个 NIST 推荐的曲线，具有广泛的兼容性，并在大多数 CPU 中提供原生支持，以实现快速计算。X25519 是一个为速度和简单性而设计的现代曲线，提供恒定时间的操作以抵抗侧信道攻击，并在低功耗或嵌入式设备上表现特别出色。

其余部分已在上面详细讨论。

```go
`tlsConfig  :=  &tls.Config{   SessionTicketsDisabled:  false,   SessionTicketKey:  [32]byte{...},   CipherSuites:  []uint16{   tls.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,   tls.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,   tls.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,   tls.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,   },   PreferServerCipherSuites:  true,   NextProtos:  []string{"h2",  "http/1.1"},   ClientAuth:  tls.RequireAndVerifyClientCert,   ClientCAs:  certPool,   VerifyPeerCertificate:  cachedCertVerifier,   MinVersion:  tls.VersionTLS12,   CurvePreferences:  []tls.CurveID{tls.CurveP256,  tls.X25519}, }` 
```

TLS 优化是关于有效平衡性能和安全的。通过通过会话重用减少握手开销，明智地选择带有 ECC 和硬件加速的加密套件，高效利用 ALPN，以及通过缓存最小化证书验证开销，Go 应用程序可以实现安全性和速度的双重目标。这种方法使 TLS 不再是性能瓶颈，而是现代网络服务的无缝安全基础。
