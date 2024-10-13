<!--yml

类别：未分类

日期：2024-10-13 06:39:02

-->

# 在 Go（Golang）中返回 JSON 体作为 HTTP 响应

> 来源：[`golangbyexample.com/json-response-body-http-go/`](https://golangbyexample.com/json-response-body-http-go/)

目录

**   概述**

+   示例*  *## **概述**

**net/http**包中 ResponseWriter 接口的**Write**方法可用于在 HTTP 响应中设置 JSON 体

在 GO 中，响应由**ResponseWriter**接口表示。这里是接口的链接 –

[`golang.org/pkg/net/http/#ResponseWriter`](https://golang.org/pkg/net/http/#ResponseWriter)

ResponseWriter 接口由 HTTP 处理程序用于构建 HTTP 响应。它提供三个函数来设置响应参数

+   头部 – 用于写入响应头

+   Write([]byte) – 用于写入响应体

+   WriteHeader(statusCode int) – 用于写入 HTTP 状态码

**Write**函数可以用于设置响应体。它接受一个字节切片作为输入。此外，还有一个**Header**函数。该函数可用于通过 Content-Type 头设置响应体的内容类型。例如，在 JSON 响应体的情况下，我们需要将 Content-Type 头设置为**“application/json”。**

```go
w.Header().Set("Content-Type", "application/json")
```

另外，请注意**WriteHeader**函数可以用于设置响应的 HTTP 状态码

## **示例**

让我们来看一个发送 HTTP 状态码和 JSON 响应体的示例

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

在上述程序中，这就是我们如何创建 JSON 响应。我们使用**json.Marshal**函数将**map[string]string**转换为 JSON 字节。

```go
resp := make(map[string]string)
resp["message"] = "Status Created"
jsonResp, err := json.Marshal(resp)
if err != nil {
	log.Fatalf("Error happened in JSON marshal. Err: %s", err)
}
w.Write(jsonResp)
```

然后它使用**Write**函数返回 JSON 响应体。上述代码返回以下 JSON 响应体作为响应

```go
{"message":"Status Created"}
```

此外，我们使用**WriteHeader**函数来指定 201 HTTP 状态码。

运行上述程序。它将在本地机器的 8080 端口启动一个服务器。现在请对服务器进行以下 curl 调用

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
< HTTP/1.1 201 Created
< Date: Sat, 10 Jul 2021 10:40:33 GMT
< Content-Length: 28
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Status Created"}
```

从输出中可以看到，它将正确返回**201**状态码以及 JSON 体。

另外，请查看我们的 Golang 高级教程系列 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
