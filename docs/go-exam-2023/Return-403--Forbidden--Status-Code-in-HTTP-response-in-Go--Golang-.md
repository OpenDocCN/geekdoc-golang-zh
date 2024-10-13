<!--yml

类别：未分类

日期：2024-10-13 06:38:37

-->

# 在 Go (Golang)中返回 403（禁止）状态码的 HTTP 响应

> 来源：[`golangbyexample.com/403-http-status-response-golang/`](https://golangbyexample.com/403-http-status-response-golang/)

目录

**   概述

+   程序*  *## **概述**

**net/http**包提供了状态码常量，可以用于返回不同的状态码-

[`golang.org/src/net/http/status.go`](https://golang.org/src/net/http/status.go)

同样的方式也可以用来返回 403（禁止）HTTP 状态码。HTTP 403 状态码由以下常量定义。

```go
http.StatusForbidden
```

在本文中，我们还将看到如何返回带有 403（禁止）状态码的 JSON 主体。

## **程序**

以下是相应的程序

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
	w.WriteHeader(http.StatusForbidden)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Forbidden"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

在这里，我们使用**WriteHeader**函数来指定 403 HTTP 状态码，并使用**Write**函数返回响应体。上述代码将以下 JSON 请求体作为响应返回。

```go
{"message":"Forbidden"}
```

运行上述程序。它将在你本地机器的 8080 端口启动一个服务器。现在进行以下 curl 调用到服务器。

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
< HTTP/1.1 403 Forbidden
< Date: Sat, 10 Jul 2021 08:16:22 GMT
< Content-Length: 23
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Forbidden"}
```

正如你从输出中看到的，它将正确返回 403 状态码及其主体。

你也可以直接将 403 传递给 WriteHeader 函数以发送 403 响应。

```go
w.WriteHeader(403)
```

这同样工作正常。试试看。

另外，查看我们的 Golang 进阶教程系列 - [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [403](https://golangbyexample.com/tag/403/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
