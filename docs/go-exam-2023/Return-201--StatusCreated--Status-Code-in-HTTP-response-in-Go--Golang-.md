<!--yml

分类：未分类

日期：2024-10-13 06:38:46

-->

# 在Go（Golang）HTTP响应中返回201（状态创建）状态码。

> 来源：[https://golangbyexample.com/201-http-status-response-golang/](https://golangbyexample.com/201-http-status-response-golang/)

目录

**[概述](#Overview "Overview")**

+   [程序](#Program "Program")*  *## **概述**

**net/http**包提供了状态码常量，可用于返回不同的状态码 - [https://golang.org/src/net/http/status.go](https://golang.org/src/net/http/status.go)

同样可以用于返回201（状态创建）HTTP状态码。HTTP 201状态码由以下常量定义。

```go
http.StatusCreated
```

在这篇文章中，我们还将看到如何返回JSON体以及201（状态创建）状态码。

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
	w.WriteHeader(http.StatusCreated)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Status Created"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

在这里，我们使用**WriteHeader**函数指定201 HTTP状态码，并使用**Write**函数返回响应体。上述代码返回以下JSON请求体作为响应。

```go
{"message":"Status Created"}
```

运行上述程序，它将在你的本地机器上启动一个8080端口的服务器。现在进行以下curl调用到服务器。

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
< HTTP/1.1 201 Created
< Date: Sat, 10 Jul 2021 10:40:33 GMT
< Content-Length: 28
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Status Created"}
```

如你所见，输出将正确返回201状态码以及主体。

你也可以直接将201传递给WriteHeader函数以发送201响应。

```go
w.WriteHeader(201)
```

这同样工作正常。试试看。

此外，查看我们的Golang进阶教程系列 - [Golang进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

+   [201](https://golangbyexample.com/tag/201/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
