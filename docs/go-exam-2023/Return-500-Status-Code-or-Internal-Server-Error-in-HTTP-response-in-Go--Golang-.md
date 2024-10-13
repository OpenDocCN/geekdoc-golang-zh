<!--yml

分类：未分类

日期：2024-10-13 06:38:50

-->

# 在Go（Golang）中返回500状态码或内部服务器错误的HTTP响应

> 来源：[https://golangbyexample.com/500-status-http-response-golang/](https://golangbyexample.com/500-status-http-response-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

**net/http**包的golang提供了状态码常量，可用于返回不同的状态码 - [https://golang.org/src/net/http/status.go](https://golang.org/src/net/http/status.go)

同样也可以用来返回500（StatusInternalServerError）HTTP状态码。HTTP 500状态码由以下常量定义。

```
http.StatusInternalServerError
```

在本文中，我们还将看到如何返回带有500（StatusInternalServerError）状态码的JSON体。

## **程序**

以下是相同的程序。

```
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

在这里，我们使用**WriteHeader**函数指定500的http状态码，并使用**Write**函数返回响应体。上述代码在响应中返回以下JSON请求体。

```
{"message":"Some Error Occurred"}
```

运行上述程序。这将在你的本地机器上启动一个8080端口的服务器。现在对服务器进行以下curl调用。

```
curl -v -X POST http://localhost:8080/example
```

以下将是输出

```
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

从输出中可以看到，它将正确返回500状态码以及响应体。

你也可以直接将500传递给WriteHeader函数以发送500响应。

```
w.WriteHeader(500)
```

这也能正常工作。试试看。

另外，查看我们的Golang进阶教程系列 - [Golang进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [500](https://golangbyexample.com/tag/500/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
