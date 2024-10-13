<!--yml

类别：未分类

日期：2024-10-13 06:39:02

-->

# 在Go（Golang）中返回JSON体作为HTTP响应

> 来源：[https://golangbyexample.com/json-response-body-http-go/](https://golangbyexample.com/json-response-body-http-go/)

目录

**   [概述](#Overview "Overview")**

+   [示例](#Example "Example")*  *## **概述**

**net/http**包中ResponseWriter接口的**Write**方法可用于在HTTP响应中设置JSON体

在GO中，响应由**ResponseWriter**接口表示。这里是接口的链接 –

[https://golang.org/pkg/net/http/#ResponseWriter](https://golang.org/pkg/net/http/#ResponseWriter)

ResponseWriter接口由HTTP处理程序用于构建HTTP响应。它提供三个函数来设置响应参数

+   头部 – 用于写入响应头

+   Write([]byte) – 用于写入响应体

+   WriteHeader(statusCode int) – 用于写入HTTP状态码

**Write**函数可以用于设置响应体。它接受一个字节切片作为输入。此外，还有一个**Header**函数。该函数可用于通过Content-Type头设置响应体的内容类型。例如，在JSON响应体的情况下，我们需要将Content-Type头设置为**“application/json”。**

```
w.Header().Set("Content-Type", "application/json")
```

另外，请注意**WriteHeader**函数可以用于设置响应的HTTP状态码

## **示例**

让我们来看一个发送HTTP状态码和JSON响应体的示例

以下是相同的程序

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

在上述程序中，这就是我们如何创建JSON响应。我们使用**json.Marshal**函数将**map[string]string**转换为JSON字节。

```
resp := make(map[string]string)
resp["message"] = "Status Created"
jsonResp, err := json.Marshal(resp)
if err != nil {
	log.Fatalf("Error happened in JSON marshal. Err: %s", err)
}
w.Write(jsonResp)
```

然后它使用**Write**函数返回JSON响应体。上述代码返回以下JSON响应体作为响应

```
{"message":"Status Created"}
```

此外，我们使用**WriteHeader**函数来指定201 HTTP状态码。

运行上述程序。它将在本地机器的8080端口启动一个服务器。现在请对服务器进行以下curl调用

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
< HTTP/1.1 201 Created
< Date: Sat, 10 Jul 2021 10:40:33 GMT
< Content-Length: 28
< Content-Type: text/plain; charset=utf-8
< 
* Connection #0 to host localhost left intact
{"message":"Status Created"}
```

从输出中可以看到，它将正确返回**201**状态码以及JSON体。

另外，请查看我们的Golang高级教程系列 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
