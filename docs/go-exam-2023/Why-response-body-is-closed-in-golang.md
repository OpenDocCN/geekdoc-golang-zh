<!--yml

类别：未分类

日期：2024-10-13 06:33:01

-->

# 为什么在golang中关闭响应体

> 来源：[https://golangbyexample.com/resposne-body-closed-golang/](https://golangbyexample.com/resposne-body-closed-golang/)

响应体应在响应完全读取后关闭。这是为了防止连接的资源泄漏。如果响应体没有关闭，则连接将不会被释放，因此无法重用。来自**http.Client**的官方文档。

来自**http.Client**的官方文档

[https://golang.org/pkg/net/http/#Client](https://golang.org/pkg/net/http/#Client)

如果Body没有被完全读取到EOF并关闭，客户端的底层RoundTripper（通常是Transport）可能无法为后续的“保持活动”请求重用与服务器的持久TCP连接。

因此，基本上，如果Body没有被完全读取并关闭，传输可能无法重用HTTP/1.x的“保持活动”TCP连接。**response.Body**实现了**io.ReadCloser**接口。

还要提到，关闭响应体的责任在于调用者。

一些重要的指导原则

+   使用defer方法来关闭响应体。这是为了确保即使在读取和解析响应时发生运行时错误，响应体也能被关闭。

+   始终先检查错误。仅在错误为nil时使用defer关闭响应体。一个nil错误总是表示一个非nil的响应体。如果错误为非nil，则处理该错误并返回。同时，请注意，关闭一个nil的响应体会导致恐慌。

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

以上是如何处理HTTP错误以及何时关闭响应体的简单示例。请注意在示例中我们使用defer函数关闭resp.Body。并且只有在错误为nil时响应体才会被关闭。

+   [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/)
