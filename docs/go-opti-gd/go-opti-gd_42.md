# 优化TLS以提升速度：握手、重用和加密套件选择

> 原文：[https://goperf.dev/02-networking/tls-for-speed/](https://goperf.dev/02-networking/tls-for-speed/)

TLS做了它应该做的事情：保持你的连接私密和可信。但它也减慢了速度——比大多数人意识到的要多得多。在Go中，如果你关心你的服务响应速度，你可以通过调整TLS协商方式和协商内容来挤出更好的性能。

## 理解TLS开销：性能受损的地方

TLS中的大多数减速都发生在开始阶段。握手是一个往返过程：选择算法、交换密钥、证明身份和设置会话。这种往返通常需要网络上的两次完整往返。在像交易平台或实时应用这样的场景中，这种延迟是明显的。

要使TLS更快，最有效的方法是减少握手步骤并降低加密工作的成本。

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

由于每个新的TLS连接都会运行整个握手过程——协商加密方式、交换密钥、验证证书——这引入了明显的延迟。会话恢复通过重用之前会话的加密状态来绕过大部分延迟，使得重新连接变得更快。

会话恢复是TLS中的一种机制，用于避免在重新连接时重复完整的握手。主要有两种方法：会话ID和会话票据。两者都依赖于服务器记住（或编码）之前连接的会话加密状态。当客户端重新连接时，它提供会话ID或会话票据，允许服务器恢复会话状态并跳过昂贵的非对称密钥交换。

会话票据是由服务器在握手结束时向客户端发出的数据块。这个票据包含加密的会话状态（例如协商的加密套件、密钥和会话参数），对客户端来说是透明的。在重新连接时，客户端将票据发送回服务器，服务器解密它以在不执行完整握手的情况下恢复会话。

在Go中，你可以通过设置会话票据密钥来启用会话恢复。服务器使用这些密钥来加密和解密客户端在恢复连接时发送回的会话状态。你可以在启动时使用crypto/rand生成一个安全的32字节密钥，如果你的服务在负载均衡器后面的多个实例上运行，你可以重用它。只需确保定期更换密钥以保持其安全性。

```go
tlsConfig := &tls.Config{
    SessionTicketsDisabled: false, // Enable session tickets explicitly
    SessionTicketKey: [32]byte{...}, // Persist securely and rotate periodically
} 
```

会话恢复之所以有效，是因为它避免了重新执行TLS中最慢的部分。不是从头开始协商一切，服务器解密票据，验证它，并恢复会话。这至少消除了一个完整的往返和所有重不对称操作，从而节省了时间和资源。

## 智能选择密码套件

密码套件定义了用于密钥交换、身份验证、加密和完整性检查的算法组合。选择高效的密码套件对性能有显著影响。

为什么选择一个而不是另一个？一些套件使用RSA进行密钥交换，这比椭圆曲线Diffie-Hellman（ECDHE）慢得多，并且缺乏前向保密性。具有AES-GCM的套件由于硬件加速，在现代CPU上运行更快，而较老的CBC模式套件则较慢且更容易出错。基于ECC的身份验证（ECDSA）与RSA相比，提供更短的签名和更低的计算成本。

要明智地选择：

+   优先选择具有ECDHE的套件以实现前向保密性和更好的性能。

+   除非针对缺乏AES加速的硬件（在这种情况下ChaCha20可能获胜），否则优先选择AES-GCM而不是AES-CBC或ChaCha20。

+   避免仅使用RSA进行密钥交换的套件，因为它们缺乏前向保密性，并且在服务器端需要更昂贵的计算。

Go标准库提供了一套安全且性能合理的默认值。对于大多数应用程序，您可以坚持使用这些默认值并且效果良好。但如果你正在为高性能环境进行调优，定义自己的首选密码套件可以让你更紧密地控制协商的内容。

优化的配置优先考虑AES-GCM的硬件加速和ECC的效率。

```go
tlsConfig := &tls.Config{
    CipherSuites: []uint16{
        tls.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
        tls.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,
        tls.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
        tls.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,
    },
    PreferServerCipherSuites: true,
} 
```

这些套件在安全性和速度之间取得了良好的平衡，利用了前向保密性和硬件加速的加密。第二个选项，TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256，当客户端可能不支持ECDSA证书时是一个合理的选择——它仍然提供了前向保密性和高效的AES-GCM加密，同时依赖于更广泛部署的RSA进行身份验证。

## 智能使用ALPN

应用层协议协商（ALPN）允许客户端和服务器在TLS握手期间就应用协议（如HTTP/2、HTTP/1.1或gRPC）达成一致，避免了建立连接后的额外往返或猜测。没有ALPN，客户端将不得不回退到较慢或效率较低的方法来检测服务器支持协议。

在Go语言中，`tls.Config`中的`NextProtos`字段定义了服务器支持的应用协议及其优先级顺序。在TLS握手过程中，客户端发送自己的列表，服务器选择双方都支持的最高优先级协议，然后确认它。

如果您想支持`HTTP/2`并回退到`HTTP/1.1`，您应该显式设置`NextProtos`，如下所示：

```go
tlsConfig := &tls.Config{
    NextProtos: []string{"h2", "http/1.1"},
} 
```

`NextProtos`中条目的顺序很重要，因为服务器按顺序遍历列表，并选择客户端也提供的第一个协议。通过将HTTP/2（`h2`）放在顶部，服务器确保在支持的情况下优先选择HTTP/2，只有在必要时才回退到HTTP/1.1。如果顺序不正确，服务器可能会选择一个较慢的协议，或者完全无法达成一致。

当`NextProtos`留空时，ALPN实际上被禁用。客户端假设HTTP/1.1，因为它在握手过程中从未看到对HTTP/2支持的指示。显式设置`NextProtos`可以宣传服务器的能力，并避免不必要的协议降级。

## 最小化证书验证开销

证书验证通常很耗时，因为它涉及几个昂贵的非对称加密操作，这直接增加了连接延迟。这种成本可以通过两种方式降低：使用更小、验证速度更快的基于ECC（ECDSA）的证书而不是RSA，以及通过缓存已验证的证书链以避免在后续连接中重复相同的验证工作。

```go
tlsConfig := &tls.Config{
    ClientAuth: tls.RequireAndVerifyClientCert,
    ClientCAs: certPool, // pre-verified CA pool
    VerifyPeerCertificate: cachedCertVerifier, // custom verifier with caching
} 
```

一个基本的缓存机制暂时存储验证结果：

```go
// Cache to avoid re-verifying the same certificate repeatedly
var verificationCache sync.Map

// cachedCertVerifier verifies a peer certificate chain and caches successful leaf fingerprints
func cachedCertVerifier(rawCerts [][]byte, verifiedChains [][]*x509.Certificate) error {
    // Compute SHA-256 fingerprint of the leaf certificate
    fingerprint := sha256.Sum256(rawCerts[0])
    if _, exists := verificationCache.Load(fingerprint); exists {
        // Already verified earlier; skip full verification
        return nil
    }

    // Parse the leaf certificate
    leafCert, err := x509.ParseCertificate(rawCerts[0])
    if err != nil {
        return fmt.Errorf("failed to parse leaf certificate: %w", err)
    }

    // Build pool of intermediate certificates provided by peer
    intermediatePool := x509.NewCertPool()
    for _, raw := range rawCerts[1:] {
        intermediateCert, err := x509.ParseCertificate(raw)
        if err != nil {
            return fmt.Errorf("failed to parse intermediate certificate: %w", err)
        }
        intermediatePool.AddCert(intermediateCert)
    }

    // Prepare verification options with trusted roots & intermediates
    opts := x509.VerifyOptions{
        Roots:         certPool,                            // trusted root CAs
        Intermediates: intermediatePool,                    // peer-provided intermediates
        KeyUsages:     []x509.ExtKeyUsage{x509.ExtKeyUsageServerAuth},
    }

    // Verify the full chain starting from leaf
    chains, err := leafCert.Verify(opts)
    if err == nil {
        // Store successful verification in cache
        verificationCache.Store(fingerprint, struct{}{})
    }
    return err
} 
```

警告

虽然这个片段说明了关键概念，但它不应直接在生产环境中使用。加密代码需要仔细分析和适应特定的环境和威胁模型。在部署之前始终验证和测试与安全相关的代码，以避免引入弱点。

## Go中的TLS最佳实践

以下配置将这些技术结合在一起，形成一个针对性能和安全性都进行了优化的tls.Config。

`MinVersion`设置告诉服务器拒绝较旧、损坏的TLS版本。任何低于TLS 1.2的版本——如SSLv3、TLS 1.0或1.1——都存在许多弱点，并且不再被认为是安全的。TLS 1.2通过引入如AES-GCM这样的AEAD密码和针对BEAST和POODLE等攻击的防御措施，解决了许多这些问题。

`CurvePreferences`字段优先考虑大多数客户端支持的快速且安全的椭圆曲线。P-256和X25519效率高，支持广泛，并且通常在硬件上加速。P-256（也称为secp256r1）是一个NIST推荐的曲线，具有广泛的兼容性，并在大多数CPU中提供原生支持，以实现快速计算。X25519是一个为速度和简单性而设计的现代曲线，提供恒定时间的操作以抵抗侧信道攻击，在低功耗或嵌入式设备上表现尤为出色。

其余部分已在上面详细讨论。

```go
tlsConfig := &tls.Config{
    SessionTicketsDisabled: false,
    SessionTicketKey: [32]byte{...},
    CipherSuites: []uint16{
        tls.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
        tls.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,
        tls.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
        tls.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,
    },
    PreferServerCipherSuites: true,
    NextProtos: []string{"h2", "http/1.1"},
    ClientAuth: tls.RequireAndVerifyClientCert,
    ClientCAs: certPool,
    VerifyPeerCertificate: cachedCertVerifier,
    MinVersion: tls.VersionTLS12,
    CurvePreferences: []tls.CurveID{tls.CurveP256, tls.X25519},
} 
```

TLS优化关乎于有效地平衡性能与安全。通过会话重用减少握手开销，明智地选择带有ECC和硬件加速的加密套件，高效利用ALPN，并通过缓存最小化证书验证开销，Go应用程序可以实现安全与速度的双重目标。这种方法使得TLS不再是性能瓶颈，而是现代网络服务的无缝安全基础。
