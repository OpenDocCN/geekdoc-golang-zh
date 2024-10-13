<!--yml

类别：未分类

日期：2024-10-13 06:38:33

-->

# 在 HTTP 响应中返回 401（未授权）状态码的 Go（Golang）实现。

> 来源：[`golangbyexample.com/401-http-status-response-go/`](https://golangbyexample.com/401-http-status-response-go/)

目录

**   概述

+   程序*  *## **概述**

**net/http**包提供状态码常量，可用于返回不同的状态码 - [`golang.org/src/net/http/status.go`](https://golang.org/src/net/http/status.go)

同样可以用于返回 401（未授权）HTTP 状态码。HTTP 401 状态码由以下常量定义。

```go
http.StatusUnauthorized
```

在本文中，我们还将看到如何在返回 401（未授权）状态码的同时返回 JSON 主体。

## **程序**

以下是相应的程序。

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
	w.WriteHeader(http.StatusUnauthorized)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Unauthorized"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

在这里，我们使用**WriteHeader**函数来指定 401 HTTP 状态码，并使用**Write**函数来返回响应主体。上述代码将返回以下 JSON 请求主体作为响应。

```go
{"message":"Unauthorized"}
```

运行上述程序。它将在本地机器的 8080 端口启动一个服务器。现在对服务器进行以下 curl 调用。

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
< HTTP/1.1 401 Unauthorized
< Date: Sat, 10 Jul 2021 08:19:52 GMT
< Content-Length: 26
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Unauthorized"}
```

如你所见，输出将正确返回**401**状态码及其主体。

你也可以直接将 401 传递给 WriteHeader 函数以发送 401 响应。

```go
w.WriteHeader(401)
```

这也可以正常工作。试试看。

同时，查看我们的 Golang 高级教程系列 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [401](https://golangbyexample.com/tag/401/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
