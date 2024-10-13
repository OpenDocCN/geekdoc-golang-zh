<!--yml

类别：未分类。

日期：2024-10-13 06:31:57。

-->

# 在 Go (Golang) 中为传入的 HTTP 请求设置响应头。

> 来源：[`golangbyexample.com/set-resposne-headers-http-go/`](https://golangbyexample.com/set-resposne-headers-http-go/)

注意：相关帖子。

+   在 Go 中从传入的 HTTP 请求中获取头部 – [`golangbyexample.com/headers-http-request-golang/`](https://golangbyexample.com/headers-http-request-golang/)

+   获取外发 HTTP 请求的响应头在 Golang 中 – [`golangbyexample.com/get-response-headers-making-go/`](https://golangbyexample.com/get-response-headers-making-go/)

+   在 Go (Golang) 中为外发 HTTP 请求设置头部 – [`golangbyexample.com/set-headers-http-request/`](https://golangbyexample.com/set-headers-http-request/)

现在让我们看看如何在传入的 HTTP 请求中设置响应头。

目录。

**概述**

+   使用 w.Header().Add() 方法.Add() 方法")。

+   使用 w.Header().Set 方法.Set 方法")。

+   示例

# **概述**。

下面是头部在 Go 中表示的格式。

```go
type Header map[string][]string
```

因此，头部是一个键值对，包含：

+   键以规范形式表示。规范形式意味着第一个字符和任何在连字符后面的字符都为大写。其他所有字符均为小写。规范形式的示例有：

```go
Content-Type
Accept-Encoding
```

+   值表示为字符串切片。为什么是字符串数组？因为在请求中同一个键可以有两个不同值的头部是完全可以接受的。两个值将被收集到切片中。

例如，如果以下头部在服务器的外发响应中设置，则：

```go
content-type: applcation/json
foo: bar1
foo: bar2
```

然后在客户端，头部将如下所示：

```go
Content-Type: applcation/json
Foo: bar1
Foo: bar2
```

注意事项：

+   **content-type** 被转换为规范形式 **Content-Type**。

+   **foo** 头部被转换为 **Foo**。还要注意，响应中会有两个相同键的头部，即 **foo**。它们的值将是 **bar1** 和 **bar2**。

现在我们已经看到头部在请求中的表示。让我们看看如何设置响应头的值。假设我们有以下需要在响应头中设置的键值对：

```go
content-type: applcation/json
foo: bar1
foo: bar2
```

在 **http.ResponseWriter** 接口中定义了一个 **Header** 方法，如下所示。它返回与响应相关联的头部。

```go
type ResponseWriter interface {
    Header() Header
    ...
}
```

在下面的示例中，假设变量 **w** 的类型为 **http.ResponseWriter**。现在让我们看看设置头部的不同方法。

# **使用 w.Header().Add() 方法**。

下面是 Add 方法的签名。

```go
func (h Header) Add(key, value string)
```

此方法用于将键值对添加到响应头中。我们已经看到上面的头部值也可以是数组。因此，此方法将附加到可能已经与该键关联的现有值上。该键也将被转换为规范形式。

例如，如果我们用不同的值添加 foo 头部两次：

```go
w.Header().Add("foo", "bar1")
w.Header().Add("foo", "bar2")
```

然后**foo**头部将被设置为**[“bar1”, “bar2”]**。同时，在向客户端发送响应时，**foo**将变为**Foo**。

# **使用 w.Header().Set 方法**

以下是该函数的签名。

```go
func (h Header) Set(key, value string)
```

它可用于设置与给定键相关的头部条目。与**Add**方法不同，此方法将替换与该键相关的任何现有值。同时，该键将转换为规范形式。

# **示例**

让我们看看一个展示以上所有要点的程序。

```go
package main

import (
	"encoding/json"
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8085", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("content-type", "application/json")

	w.Header().Add("foo", "bar1")
	w.Header().Add("foo", "bar2")

	resp := make(map[string]string)
	resp["message"] = "success"
	jsonResp, _ := json.Marshal(resp)
	w.Write(jsonResp)
}
```

在上述程序中，我们启动了一个监听在 8085 端口的服务器。我们将**content-type**头部设置如下。

```go
w.Header().Set("content-type", "application/json")
```

我们将**foo**头部设置如下。

```go
w.Header().Add("foo", "bar1")
w.Header().Add("foo", "bar2")
```

我们还在该端点定义了一个 URL。运行此服务器并进行以下 API 调用。

```go
curl -v -X POST http://localhost:8085/example
```

运行此 API 后，请在终端检查响应头。它与我们讨论的内容完全一致。此外，每个头部键都转换为其规范形式。

```go
< HTTP/1.1 200 OK
< Content-Type: application/json
< Foo: bar1
< Foo: bar2
< Content-Length: 21
< 
* Connection #0 to host localhost left intact
{"message":"success"}
```

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
