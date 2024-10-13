<!--yml

类别：未分类。

日期：2024-10-13 06:39:25

-->

# 在 Go（Golang）中以 HTTP 响应返回图像或文件。

> 来源：[`golangbyexample.com/image-http-response-golang/`](https://golangbyexample.com/image-http-response-golang/)

目录

**概述**

+   示例*  *## **概述**

**ResponseWriter** 接口的 **Write** 方法可以用于在 HTTP 响应体中发送 **image** 或 **file**。当我们将文件或图像作为 HTTP 响应的一部分发送时，Content-Type 响应头为

```go
application/octet-stream
```

在 Go 中，响应由 **ResponseWriter** 接口表示。这里是接口的链接 –

```go
https://golang.org/pkg/net/http/#ResponseWriter
```

ResponseWriter 接口由 HTTP 处理程序用于构造 HTTP 响应。它提供了三个函数来设置响应参数。

+   Header – 用于写入响应头。

+   Write([]byte) – 用于写入响应体。

+   WriteHeader(statusCode int) – 用于写入 HTTP 状态码。

**Write** 函数可用于设置响应体。它接受一个字节切片作为输入。因此，我们需要先将文件或图像读取到字节切片中，然后再将其作为参数传递给 **Write** 函数。为此，我们将使用 **ioutil** 包提供的 **ReadFile** 函数。此外，还有一个 **Header** 函数。此函数可用于使用 Content-Type 头设置响应体的内容类型。

```go
w.Header().Set("Content-Type", "application/octet-stream")
```

此外，请注意 **WriteHeader** 函数可用于设置响应的 HTTP 状态码。

## **示例**

让我们看看如何将文件或图像作为 HTTP 响应的一部分发送的示例。下面是相应的程序。

```go
package main

import (
	"io/ioutil"
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/photo", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
	fileBytes, err := ioutil.ReadFile("test.png")
	if err != nil {
		panic(err)
	}
	w.WriteHeader(http.StatusOK)
	w.Header().Set("Content-Type", "application/octet-stream")
	w.Write(fileBytes)
	return
}
```

这就是我们如何在 Go 中将文件或图像读取到变量中的。

```go
fileBytes, err := ioutil.ReadFile("test.png")
```

然后我们将这个变量传递给 **Write** 函数。

```go
w.Write(fileBytes)
```

请注意，在上面的程序中，我们发送的是本地机器上存在的 **test.png**。您可以将其替换为您机器上存在的任何其他文件，可以是 png 或其他文件。

此外，我们正在使用 **WriteHeader** 函数来指定 **200** HTTP 状态码。我们还设置了正确的头部。

```go
w.Header().Set("Content-Type", "application/octet-stream")
```

运行上面的程序。它将在您本地机器的 8080 端口启动一个服务器。现在向服务器发出下面的 curl 调用。请注意在 curl 调用中，我们还将输出保存到本地文件。

```go
curl -v -X POST http://localhost:8080/example > temp.png
```

输出将类似于下面的内容。

```go
* Connected to localhost (::1) port 8080 (#0)
> POST /example HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Sat, 10 Jul 2021 19:32:47 GMT
< Content-Type: image/png
< Transfer-Encoding: chunked
< 
{ [5846 bytes data]
100  5833    0  5833    0     0   957k      0 --:--:-- --:--:-- --:--:-- 1139k
```

现在检查您从中发出 curl 调用的当前目录。在该目录下会有一个名为 **test.png** 的文件，这就是服务器发送的相同文件。

此外，请查看我们的 Golang 高级教程系列 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
