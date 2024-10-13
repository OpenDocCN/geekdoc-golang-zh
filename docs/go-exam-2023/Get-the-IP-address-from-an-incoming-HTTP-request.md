<!--yml

category: 未分类

日期：2024-10-13 06:10:40

-->

# 从传入的 HTTP 请求中获取 IP 地址。

> 来源：[`golangbyexample.com/golang-ip-address-http-request/`](https://golangbyexample.com/golang-ip-address-http-request/)

目录

**   概述

+   代码*  *# **概述**

在这篇文章中，我们将使用传入的 HTTP 请求获取客户端的 IP 地址。

+   **X-REAL-IP** 头部。

+   如果 **X-REAL-IP** 为空，我们将退回到 **X-FORWARDED-FOR** 头部。

+   如果 **X-FORWARDED-FOR** 为空，我们将退回到 http.Request 结构的 **RemoteAddr**。

请注意，

+   **X-REAL-IP** 头部仅包含客户端机器的一个 IP 地址。一些代理服务器，如 Nginx，如果该头部为空，会根据请求之前遇到的可信代理填充此头部。同时请注意，客户端也可以轻易伪造该头部。

+   **X-FORWARDED-FOR** 是一个 IP 地址列表——代理链。将其视为请求跳跃日志。因此，如果请求源自 IP 为 **ip1** 的客户端，然后经过 IP 为 **ip2** 的代理服务器，再经过 IP 为 **ip3** 的负载均衡器，那么 **X-FORWARDED-FOR** 的值将是 **“ip1,ip2,ip3”**。因此，按 **“,”** 分割是个好主意。此外，请注意，客户端也可以轻易伪造它。只有在你控制设置该头部的代理时，才应该使用该头部。

+   **RemoteAddr** 包含客户端的真实 IP 地址。它是网络服务器接收连接的实际物理 IP 地址，响应将发送到该地址。但如果客户端通过代理连接，它将给出代理的 IP 地址。如果使用负载均衡器或反向代理服务器，则将提供它们的地址。**RemoteAddr** 代表 IP 端口组合。

有关 **X-REAL-IP** 和 **X-FORWARDED-FOR** 头部的更多信息，请参考这篇文章。

[`distinctplace.com/2014/04/23/story-behind-x-forwarded-for-and-x-real-ip-headers/`](https://distinctplace.com/2014/04/23/story-behind-x-forwarded-for-and-x-real-ip-headers/)

在上述三个值中，**RemoteAddr** 是最可靠的，但如果客户端位于代理后面，或使用负载均衡器或反向代理服务器，它将永远无法提供正确的 IP 地址，因此顺序为 **X-REAL-IP**，然后是 **X-FORWARDED-FOR**，最后是 **RemoteAddr**。请注意，恶意用户仍然可以伪造 **X-REAL-IP** 和 **X-FORWARDED-FOR** 头部。

# **代码**

让我们看看在 GOLANG 中的工作代码。

```go
package main

import (
    "fmt"
    "net"
    "net/http"
    "strings"
)

func main() {
    handlerFunc := http.HandlerFunc(handleRequest)
    http.Handle("/getIp", handlerFunc)
    http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
    ip, err := getIP(r)
    if err != nil {
        w.WriteHeader(400)
        w.Write([]byte("No valid ip"))
    }
    w.WriteHeader(200)
    w.Write([]byte(ip))
}

func getIP(r *http.Request) (string, error) {
    //Get IP from the X-REAL-IP header
    ip := r.Header.Get("X-REAL-IP")
    netIP := net.ParseIP(ip)
    if netIP != nil {
        return ip, nil
    }

    //Get IP from X-FORWARDED-FOR header
    ips := r.Header.Get("X-FORWARDED-FOR")
    splitIps := strings.Split(ips, ",")
    for _, ip := range splitIps {
        netIP := net.ParseIP(ip)
        if netIP != nil {
            return ip, nil
        }
    }

    //Get IP from RemoteAddr
    ip, _, err := net.SplitHostPort(r.RemoteAddr)
    if err != nil {
        return "", err
    }
    netIP = net.ParseIP(ip)
    if netIP != nil {
        return ip, nil
    }
    return "", fmt.Errorf("No valid ip found")
}
```

发起一个 curl 调用。

```go
curl -v -X  GET http://localhost:8080/getIp
```

**输出：**

```go
On my machine it outputs ::1 which is actually loopback address for IPV6
```

在你的机器上，它将输出 IPV4 的 127.0.0.1 或 IPV6 的 ::1。

[`en.wikipedia.org/wiki/Localhost`](https://en.wikipedia.org/wiki/Localhost)

+   [golang](https://golangbyexample.com/tag/golang/)*
