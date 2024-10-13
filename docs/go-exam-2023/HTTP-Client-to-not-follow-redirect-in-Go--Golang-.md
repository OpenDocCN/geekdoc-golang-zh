<!--yml

分类：未分类

日期：2024-10-13 06:34:11

-->

# HTTP 客户端在 Go (Golang) 中不跟随重定向。

> 来源：[`golangbyexample.com/http-no-redirect-client-golang/`](https://golangbyexample.com/http-no-redirect-client-golang/)

目录

+   概述

+   CheckDirect 是 nil

+   CheckDirect 是非 nil

# **概述**

**http.Client** 结构用于在 golang 中发起 HTTP 请求。

[`golang.org/src/net/http/client.go`](https://golang.org/src/net/http/client.go)

**http.Client** 让你指定重定向处理的策略。以下是 **http.Client** 结构的结构。

```go
type Client struct {
	Transport RoundTripper
	CheckRedirect func(req *Request, via []*Request) error
	Jar CookieJar
	Timeout time.Duration
}
```

**CheckRedirect** 字段实际上是一个函数，它允许你指定处理重定向的方式。此函数在跟随任何未来的重定向之前被调用。因此，基本上，每当服务器响应重定向时，客户端会首先调用 **CheckRedirect** 函数以检查是否应跟随重定向。以下是 **CheckRedirect** 函数的签名。

```go
func(req *Request, via []*Request) error
```

+   第一个参数 **req** 是即将到来的请求。

+   第二个参数 **via** 是已经发出的请求。这就是它是一个切片的原因。第一个元素是最旧的请求。

有两种情况需要考虑。

# **CheckDirect 是 nil**

在初始化 **http.Client** 结构时，你不指定 CheckRedirect 字段。在这种情况下，将启用默认行为，HTTP 客户端将跟随 10 次重定向，然后返回错误。以下是 **defaultCheckRedirect** 函数，它在尝试 10 次后退出。

```go
func defaultCheckRedirect(req *Request, via []*Request) error {
	if len(via) >= 10 {
		return errors.New("stopped after 10 redirects")
	}
	return nil
}
```

# **CheckDirect 是非 nil**

在初始化 **http.Client** 结构时，你指定自己的 **CheckRedirect** 函数。当你指定此函数时，它将在跟随任何未来的重定向之前被调用。当你编写自己的 CheckRedirect 函数时，需要记住以下内容。

当 **CheckRedirect** 函数返回错误时，HTTP 客户端将返回两件事。

+   上一个响应以及请求体已关闭。

+   实际错误（包装在 **url.Error** 中）。

**还有一种特殊情况，当 **CheckRedirect** 函数返回 **ErrUseLastResponse** 错误时。在这种情况下，客户端将返回。**

***   最近的响应以及未关闭的请求体。

+   非 nil 错误

所以，基本上当你不想让 HTTP 客户端跟随重定向时，请指定自己的 **CheckRedirect** 函数以控制行为。例如，以下是一个示例 **CheckRedirect** 函数，它会通知 HTTP 客户端不跟随任何重定向，并且不返回任何错误，仅返回最后的响应以及未关闭的请求体。

```go
func MyCheckRedirect(req *Request, via []*Request) error {
	return http.ErrUseLastResponse
}
```

以下是 http 包中 **ErrUseLastResponse** 的定义。

```go
var ErrUseLastResponse = errors.New("net/http: use last response")
```

创建 **http.Client** 如下，指定 **CheckRedirect** 函数。

```go
client := &http.Client{
    CheckRedirect: func(req *Request, via []*Request) error {
	return http.ErrUseLastResponse
    },
}
```

这是另一个示例 **CheckRedirect** 函数，它将只跟随重定向两次，然后返回错误。

```go
func MyCheckRedirect(req *Request, via []*Request) error {
	if len(via) >= 2 {
		return errors.New("stopped after 10 redirects")
	}
	return nil
}
```

+   [重定向](https://golangbyexample.com/tag/redirect/)***
