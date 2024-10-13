<!--yml

类别：未分类

日期：2024-10-13 06:38:42

-->

# 在 Go (Golang) 中返回 200 (状态正常) 状态码的 HTTP 响应 -P

> 来源：[`golangbyexample.com/200-http-status-response-golang/`](https://golangbyexample.com/200-http-status-response-golang/)

目录

+   概述

+   程序

## **概述**

Golang 的 **net/http** 包提供了可以用来返回不同状态码的状态码常量 - [`golang.org/src/net/http/status.go`](https://golang.org/src/net/http/status.go)

同样可以用来返回 200 (状态正常) HTTP 状态码。 HTTP 200 状态码由以下常量定义。

```go
http.StatusOK
```

在这篇文章中，我们还将看到如何返回 JSON 主体以及 200 (状态正常) 状态码。

## **程序**

以下是相同的程序

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
	w.WriteHeader(http.StatusOK)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Status OK"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

在这里，我们使用 **WriteHeader** 函数来指定 200 HTTP 状态码，并使用 **Write** 函数返回响应主体。上述代码将以下 JSON 请求主体作为响应返回。

```go
{"message":"Status OK"}
```

运行上述程序。这将在你的本地机器上启动一个 8080 端口的服务器。现在对服务器进行以下 curl 调用。

```go
curl -v -X POST http://localhost:8080/example
```

以下将是输出。

```go
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Sat, 10 Jul 2021 09:42:59 GMT
< Content-Length: 23
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Status OK"}
```

正如你从输出中看到的，它将正确返回 200 状态码以及主体。

你也可以直接将 200 传递给 WriteHeader 函数以发送 200 响应。

```go
w.WriteHeader(200)
```

这同样有效。试试看。

同时，请查看我们的 Golang 高级教程系列 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [200](https://golangbyexample.com/tag/200/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
