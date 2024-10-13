<!--yml

类别：未分类

日期：2024-10-13 06:38:29

-->

# 在 Go（Golang）中返回 404（资源未找到）HTTP 响应状态码。

> 来源：[`golangbyexample.com/404-http-status-code-golang/`](https://golangbyexample.com/404-http-status-code-golang/)

目录

**   概述

+   程序*  *## **概述**

golang 的 HTTP 包提供了可以用于返回不同状态码的状态码常量 - [`golang.org/src/net/http/status.go`](https://golang.org/src/net/http/status.go)

同样也可以用于返回 404（资源未找到）HTTP 状态码。HTTP 404 状态码由下面的常量定义。

```go
http.StatusNotFound
```

在本文中，我们还将看到如何在返回 404（资源未找到）HTTP 状态码时返回 JSON 正文。

## **程序**

下面是相同的程序。

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
	w.WriteHeader(http.StatusNotFound)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Resource Not Found"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

在这里，我们使用**WriteHeader**函数指定 404 HTTP 状态码，并使用**Write**函数返回响应正文。上述代码将下面的 JSON 请求正文返回作为响应。

```go
{"message":"Resource Not Found"}
```

运行上述程序。它将在你的本地机器上启动一个 8080 端口的服务器。现在对服务器进行如下 curl 调用。

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
< HTTP/1.1 404 Not Found
< Date: Sat, 10 Jul 2021 06:18:12 GMT
< Content-Length: 32
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Resource Not Found"}
```

从输出中可以看出，它正确地返回了**404**状态码及其正文。

你也可以直接将 404 传递给**WriteHeader**函数来发送 404 响应。

```go
w.WriteHeader(404)
```

这也能正常工作。试试看。

golang 的**net/http**包也提供了一个**"NotFound"**处理程序，可以用于每次返回特定 API 的 404。

[`golang.org/pkg/net/http/#NotFound`](https://golang.org/pkg/net/http/#NotFound)

此处理程序函数返回 404 状态及下面的响应正文。

```go
404 page not found
```

下面是相同的简单程序。

```go
package main

import (
	"net/http"
)

func main() {
	http.HandleFunc("/example", http.NotFound)
	http.ListenAndServe(":8080", nil)
}
```

在上述程序中，我们简单地为**localhost:8080/example** API 指定了 NotFound 处理程序，如下所示。

```go
http.HandleFunc("/example", http.NotFound)
```

运行上述程序。它将在你的本地机器上启动一个 8080 端口的服务器。现在对服务器进行如下 curl 调用。

```go
curl -v -X POST http://localhost:8080/example
```

```go
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 404 Not Found
< Content-Type: text/plain; charset=utf-8
< X-Content-Type-Options: nosniff
< Date: Sat, 10 Jul 2021 06:23:41 GMT
< Content-Length: 19
< 
404 page not found
* Connection #0 to host localhost left intact
```

此外，请查看我们的 Golang 高级教程系列 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [404](https://golangbyexample.com/tag/404/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
