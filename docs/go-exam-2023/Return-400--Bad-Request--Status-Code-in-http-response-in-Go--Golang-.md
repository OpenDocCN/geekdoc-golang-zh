<!--yml

类别: 未分类

日期: 2024-10-13 06:38:24

-->

# 在 Go（Golang）中返回 400（错误请求）状态码的 HTTP 响应

> 来源：[`golangbyexample.com/400-http-status-response-golang/`](https://golangbyexample.com/400-http-status-response-golang/)

目录

**   概述

+   程序*  *## **概述**

Golang 的**net/http**包提供了状态码常量，可用于返回不同的状态码 - [`golang.org/src/net/http/status.go`](https://golang.org/src/net/http/status.go)

同样也可以用来返回 400（错误请求）HTTP 状态码。HTTP 400 状态码由以下常量定义。

在这篇文章中，我们还将看到如何在返回 400（错误请求）状态码的同时返回 JSON 主体。

## **程序**

以下是相同的程序

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
	w.WriteHeader(http.StatusBadRequest)
	return
}
```

这里我们使用**WriteHeader**函数来指定 HTTP 状态码。

运行上述程序。它将在你的本地机器上启动一个 8080 端口的服务器。现在向服务器发出以下 curl 调用。

```go
curl -v -X POST http://localhost:8080/example
```

以下将是输出

```go
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 400 Bad Request
< Date: Sat, 10 Jul 2021 05:50:32 GMT
< Content-Length: 0
< 
* Connection #0 to host localhost left intact
```

从输出中可以看到，它将正确返回 400 状态码。如果我们还想返回 JSON 错误主体，那么以下是相应的代码。

```go
package main

import (
	"encoding/json"
	"log"
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
	w.WriteHeader(http.StatusBadRequest)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Bad Request"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

上述代码在响应中返回以下 JSON 请求主体。

```go
{"message":"Bad Request"}
```

运行上述程序。它将在你的本地机器上启动一个 8080 端口的服务器。现在向服务器发出以下 curl 调用。

```go
curl -v -X POST http://localhost:8080/example
```

以下将是输出

```go
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 400 Bad Request
< Date: Sat, 10 Jul 2021 05:58:42 GMT
< Content-Length: 25
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Bad Request"}
```

从输出中可以看到，它正确返回了 400 状态码及其主体。

你也可以直接将 400 传递给 WriteHeader 函数以发送 400 响应。

```go
w.WriteHeader(400)
```

这也能正常工作。试试看。

另外，查看我们的 Golang 进阶教程系列 - [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [400](https://golangbyexample.com/tag/400/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
