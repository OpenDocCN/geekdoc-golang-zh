<!--yml

类别: 未分类

日期: 2024-10-13 06:38:57

-->

# 如何在Go（Golang）中设置HTTP响应状态码

> 来源：[https://golangbyexample.com/set-http-status-code-golang/](https://golangbyexample.com/set-http-status-code-golang/)

目录

**   [概述](#Overview "概述")

+   [示例](#Example "示例")*  *## **概述**

**WriteHeader**方法在**net/http**包中的ResponseWriter接口可以用于从Golang服务器返回状态码。

在Go中，响应由**ResponseWriter**接口表示。这里是接口的链接 –

[https://golang.org/pkg/net/http/#ResponseWriter](https://golang.org/pkg/net/http/#ResponseWriter)

ResponseWriter接口由HTTP处理程序用于构建HTTP响应。它提供了三个函数来设置响应参数

+   Header – 用于写入响应头

+   Write([]byte) – 用于写入响应正文

+   WriteHeader(statusCode int) – 用于写入HTTP状态码

可以看到，**WriteHeader**函数以状态码作为输入，该状态码会在HTTP响应中发送。而**Write**函数则可以用于设置响应正文。需要注意的是，如果没有显式调用**WriteHeader**，则对**Write**函数的调用会内部调用**WriteHeader**函数，并返回状态码200，即StatusOk。

## **示例**

让我们看看发送HTTP状态码和正文的示例

以下是相同程序的内容

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

在这里，我们使用**WriteHeader**函数指定201 HTTP状态码。同样，我们可以将此处列出的任何状态码传递给**WriteHeader**函数

[https://golang.org/src/net/http/status.go](https://golang.org/src/net/http/status.go)

它还使用**Write**函数返回响应正文。上述代码将以下JSON请求正文作为响应返回

```
{"message":"Status Created"}
```

运行上述程序。这将在你本地机器的8080端口上启动一个服务器。现在可以向服务器发出以下curl调用

```
curl -v -X POST http://localhost:8080/example
```

下面是输出结果

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

从输出中可以看到，它会正确返回**201**状态码及其正文。

你也可以直接将201传递给WriteHeader函数，以发送201响应。

```
w.WriteHeader(201)
```

尝试一下，它会有效。

我们提到过，我们并没有显式调用WriteHeader，因此对**Write**函数的调用将内部调用WriteHeader函数，并返回状态码200，即StatusOk。我们来看看一个例子-

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
	w.Header().Set("Content-Type", "application/json")
	resp := make(map[string]string)
	resp["message"] = "Success"
	jsonResp, err := json.Marshal(resp)
	if err != nil {
		log.Fatalf("Error happened in JSON marshal. Err: %s", err)
	}
	w.Write(jsonResp)
	return
}
```

查看上面的代码。我们没有在任何地方调用**WriteHeader**函数。因此，程序应该默认发送状态码200。

运行上述程序。这将在你本地机器的8080端口上启动一个服务器。现在可以向服务器发出以下curl调用

```
curl -v -X POST http://localhost:8080/example
```

下面是输出结果

```
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Content-Type: application/json
< Date: Sat, 10 Jul 2021 16:24:11 GMT
< Content-Length: 21
< 
* Connection #0 to host localhost left intact
{"message":"Success"}
```

从输出中可以注意到，它返回了200状态码。

同时，请查看我们的Golang高级教程系列 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [去](https://golangbyexample.com/tag/go/)*   [戈Lang](https://golangbyexample.com/tag/golang/)*
