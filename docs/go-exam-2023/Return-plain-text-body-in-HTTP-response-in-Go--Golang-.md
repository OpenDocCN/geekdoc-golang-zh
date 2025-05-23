<!--yml

分类：未分类

日期：2024-10-13 06:39:17

-->

# 在 Go（Golang）中返回纯文本体的 HTTP 响应。

> 来源：[`golangbyexample.com/plain-text-response-body-golang/`](https://golangbyexample.com/plain-text-response-body-golang/)

目录

+   概述

+   示例

## **概述**

**Write**方法的 ResponseWriter 接口在**net/http**包中可用于在 HTTP 响应中设置**text/plain**体。

在 GO 中，响应由**ResponseWriter**接口表示。接口的链接在这里 – [`golang.org/pkg/net/http/#ResponseWriter`](https://golang.org/pkg/net/http/#ResponseWriter)

ResponseWriter 接口由 HTTP 处理程序用于构造 HTTP 响应。它提供三个函数来设置响应参数。

+   Header – 用于写入响应头。

+   Write([]byte) – 用于写入响应体。

+   WriteHeader(statusCode int) – 用于写入 HTTP 状态码。

**Write**函数可用于设置响应体。它接受一个字节切片作为输入。此外，还有一个**Header**函数。此函数可用于通过 Content-Type 头设置响应体的内容类型。例如，对于 text/plain 响应体，我们需要将 Content-Type 头设置为**“text/plain”。**

```go
w.Header().Set("Content-Type", "text/plain")
```

此外，请注意**WriteHeader**函数可用于设置响应的 HTTP 状态码。

## **示例**

让我们看看发送 HTTP 状态码和**text/plain**响应体的示例。

下面是相应的程序。

```go
package main

import (
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
	w.WriteHeader(http.StatusOK)
	w.Header().Set("Content-Type", "application/text")
	w.Write([]byte("Success"))
	return
}
```

我们使用**Write**函数返回 text/plain 响应体。上述代码返回以下**text/plain**体作为响应。

```go
Success
```

此外，我们正在使用**WriteHeader**函数来指定**200**的 HTTP 状态码。我们还设置了正确的头信息。

运行上述程序。它将在本地机器的 8080 端口启动一个服务器。现在对服务器进行以下 curl 调用。

```go
curl -v -X POST http://localhost:8080/example
```

下面将是输出。

```go
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Sat, 10 Jul 2021 19:01:56 GMT
< Content-Length: 7
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
Success
```

从输出中可以看到，它将正确返回**200**状态码以及**text/plain**体。此外，**Content-Type**响应头被设置为**text/plain**

此外，请查看我们的 Golang 进阶教程系列 - [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)


