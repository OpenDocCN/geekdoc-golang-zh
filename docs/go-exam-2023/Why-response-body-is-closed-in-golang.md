<!--yml

类别：未分类

日期：2024-10-13 06:33:01

-->

# 为什么在 golang 中关闭响应体

> 来源：[`golangbyexample.com/resposne-body-closed-golang/`](https://golangbyexample.com/resposne-body-closed-golang/)

响应体应在响应完全读取后关闭。这是为了防止连接的资源泄漏。如果响应体没有关闭，则连接将不会被释放，因此无法重用。来自**http.Client**的官方文档。

来自**http.Client**的官方文档

[`golang.org/pkg/net/http/#Client`](https://golang.org/pkg/net/http/#Client)

如果 Body 没有被完全读取到 EOF 并关闭，客户端的底层 RoundTripper（通常是 Transport）可能无法为后续的“保持活动”请求重用与服务器的持久 TCP 连接。

因此，基本上，如果 Body 没有被完全读取并关闭，传输可能无法重用 HTTP/1.x 的“保持活动”TCP 连接。**response.Body**实现了**io.ReadCloser**接口。

还要提到，关闭响应体的责任在于调用者。

一些重要的指导原则

+   使用 defer 方法来关闭响应体。这是为了确保即使在读取和解析响应时发生运行时错误，响应体也能被关闭。

+   始终先检查错误。仅在错误为 nil 时使用 defer 关闭响应体。一个 nil 错误总是表示一个非 nil 的响应体。如果错误为非 nil，则处理该错误并返回。同时，请注意，关闭一个 nil 的响应体会导致恐慌。

一个简单的示例

```go
resp, err := http.Get("http://google.com/")
if err != nil {
    //Handle the error here.
    return
}
defer resp.Body.Close()
//Read and parse response body here
```

以上是如何处理 HTTP 错误以及何时关闭响应体的简单示例。请注意在示例中我们使用 defer 函数关闭 resp.Body。并且只有在错误为 nil 时响应体才会被关闭。

+   [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/)
