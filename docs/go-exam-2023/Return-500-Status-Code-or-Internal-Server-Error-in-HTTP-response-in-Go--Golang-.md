<!--yml

分类：未分类

日期：2024-10-13 06:38:50

-->

# 在 Go（Golang）中返回 500 状态码或内部服务器错误的 HTTP 响应

> 来源：[`golangbyexample.com/500-status-http-response-golang/`](https://golangbyexample.com/500-status-http-response-golang/)

目录

+   概述

+   程序

## **概述**

**net/http**包的 golang 提供了状态码常量，可用于返回不同的状态码 - [`golang.org/src/net/http/status.go`](https://golang.org/src/net/http/status.go)

同样也可以用来返回 500（StatusInternalServerError）HTTP 状态码。HTTP 500 状态码由以下常量定义。

```go
http.StatusInternalServerError
```

在本文中，我们还将看到如何返回带有 500（StatusInternalServerError）状态码的 JSON 体。

## **程序**

以下是相同的程序。

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
	w.WriteHeader(http.StatusInternalServerError)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Some Error Occurred"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

在这里，我们使用**WriteHeader**函数指定 500 的 http 状态码，并使用**Write**函数返回响应体。上述代码在响应中返回以下 JSON 请求体。

```go
{"message":"Some Error Occurred"}
```

运行上述程序。这将在你的本地机器上启动一个 8080 端口的服务器。现在对服务器进行以下 curl 调用。

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
< HTTP/1.1 500 Internal Server Error
< Date: Sat, 10 Jul 2021 10:49:52 GMT
< Content-Length: 33
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Some Error Occurred"}
```

从输出中可以看到，它将正确返回 500 状态码以及响应体。

你也可以直接将 500 传递给 WriteHeader 函数以发送 500 响应。

```go
w.WriteHeader(500)
```

这也能正常工作。试试看。

另外，查看我们的 Golang 进阶教程系列 - [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [500](https://golangbyexample.com/tag/500/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
