<!--yml

分类：未分类

date: 2024-10-13 06:32:13

-->

# 在 Go (Golang) 中，从传入的 HTTP 请求中获取客户端的用户代理。

> 来源：[https://golangbyexample.com/user-agent-http-golang/](https://golangbyexample.com/user-agent-http-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

在传入的 HTTP 请求中，用户代理存在于请求的头部。在 Go 中，传入的 HTTP 请求由 **http.Request** 结构体表示。

[https://golang.org/src/net/http/request.go](https://golang.org/src/net/http/request.go)

**http.Request** 结构体提供了获取请求用户代理的以下方法。

```go
func (r *Request) UserAgent() string
```

此方法需要在请求对象上调用。

# **示例**

让我们看看相应的程序。

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {

	userAgent := r.UserAgent()
	fmt.Printf("UserAgent:: %s", userAgent)
}
```

运行上述程序并进行以下 API 调用。同时传入用户代理头部。

```go
curl -v -X POST http://localhost:8080/example -H "user-agent: Mozialla -1.0"
```

**输出**

```go
UserAgent:: Mozialla -1.0
```

在上述程序中，我们在请求结构体对象上调用 **UserAgent()** 函数，然后打印出来。它正确地打印了用户代理。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
