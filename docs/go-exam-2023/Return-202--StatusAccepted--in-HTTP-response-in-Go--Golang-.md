<!--yml

分类：未分类

日期：2024-10-13 06:39:10

-->

# 在Go（Golang）中返回202（StatusAccepted）HTTP响应

> 来源：[https://golangbyexample.com/202-status-http-response-go/](https://golangbyexample.com/202-status-http-response-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

**net/http**包提供了状态码常量，可以用于返回不同的状态码 - [https://golang.org/src/net/http/status.go](https://golang.org/src/net/http/status.go)

同样也可以用于返回202（StatusAccepted）HTTP状态码。HTTP 202状态码由下面的常量定义。

```
http.StatusAccepted
```

在本文中，我们还将看到如何返回带有202（StatusAccepted）状态码的JSON主体

## **程序**

下面是相应的程序

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
	w.WriteHeader(http.StatusAccepted)
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Status Accepted"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

在这里我们使用**WriteHeader**函数来指定202 HTTP状态码，并使用**Write**函数返回响应主体。上述代码将以下JSON请求主体返回作为响应

```
{"message":"Status Accepted"}
```

运行上述程序。它将在你的本地机器上启动一个8080端口的服务器。现在对服务器进行下面的curl调用。

```
curl -v -X POST http://localhost:8080/example
```

下面将是输出结果

```
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 202 Accepted
< Date: Sat, 10 Jul 2021 17:42:27 GMT
< Content-Length: 29
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Status Accepted"}
```

正如你从输出中看到的，它将正确返回202状态码以及主体。

你也可以直接将202传递给WriteHeader函数以发送202响应。

```
w.WriteHeader(202)
```

这同样有效。试试看。

另外，查看我们的Golang高级教程系列 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
