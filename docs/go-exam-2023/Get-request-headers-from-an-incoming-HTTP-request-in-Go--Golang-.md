<!--yml

分类：未分类

日期：2024-10-13 06:31:28

-->

# 在 Go (Golang) 中从传入的 HTTP 请求获取请求头

> 来源：[https://golangbyexample.com/headers-http-request-golang/](https://golangbyexample.com/headers-http-request-golang/)

注意：相关帖子

+   在 Go 中为传入 HTTP 请求设置响应头 - [https://golangbyexample.com/set-resposne-headers-http-go/](https://golangbyexample.com/set-resposne-headers-http-go/)

+   在 Golang 中获取传出 HTTP 请求的响应头 - [https://golangbyexample.com/get-response-headers-making-go/](https://golangbyexample.com/get-response-headers-making-go/)

+   在 Go (Golang) 中为传出的 HTTP 请求设置头部– [https://golangbyexample.com/set-headers-http-request/](https://golangbyexample.com/set-headers-http-request/)

现在我们来看看如何在传入的 HTTP 请求中获取请求头

目录

**   [概述](#Overview "概述")

+   [使用 Header.Values 方法](#Using_HeaderValues_method "使用 Header.Values 方法")

+   [使用 Header.Get 方法](#Using_HeaderGet_method "使用 Header.Get 方法")

+   [直接访问 Header 结构](#Directly_Accessing_the_Header_Struct "直接访问 Header 结构")

+   [示例](#Example "示例")*  *# **概述**

以下是 Go 中头部的表示格式。

```go
type Header map[string][]string
```

因此，头部是一个键值对

+   键以规范形式表示。规范形式意味着第一个字符和破折号后面的任何字符都为大写。其他所有字符均为小写。规范形式的示例包括

```go
Content-Type
Accept-Encoding
```

+   值表示为字符串切片。为什么是字符串数组？因为在请求中有两个具有相同键和不同值的头部是完全可以接受的。这两个值将被收集在切片中。

例如，如果在传入请求中有以下头部

```go
content-type: applcation/json
foo: bar1
foo: bar2
```

然后在服务器上，头部看起来像

```go
map[string][]string {
  "Content-Type": {"application/json"},
  "Foo": {"bar1" , "bar2"}
}
```

注意：

+   **content-type** 被转换为规范形式 **Content-Type**

+   **content-type** 头部表示为一个包含 1 个元素 **application/json** 的字符串数组

+   **foo** 头部表示为一个包含两个元素 **bar1** 和 **bar2** 的字符串数组

现在我们已经看到请求中的头部是如何表示的。让我们看看如何根据键获取头部值。假设我们有以下头部的键值对

```go
content-type: applcation/json
foo: bar1In the below example let's assume that variable r of type *http.Request type. Now let's see different ways of getting a header value.
foo: bar2
```

**Header** 在 **http.Request** 结构中如下所示。

```go
type Request struct {
    Method string
    Header Header
    ...
}
```

在下面的示例中，假设变量 **r** 的类型为 ***http.Request**。现在让我们看看根据键获取头部值的不同方式

# **使用 Header.Values 方法**

用于访问给定键的头部的所有值，使用 Values 方法。下面是该方法的格式

```go
func (h Header) Values(key string) []string
```

注意返回类型是字符串切片。它将返回与给定头部键相关的所有值。同时，在获取值之前，给定的键会首先转换为规范形式。可以如下调用

```go
r.Header.Values(key)
```

例如对于

+   **r.Header.Values("content-type")** 将输出 **“application/json”**

+   **r.Header.Values(“foo”)**将输出**[“bar1”, “bar2”]**。

# **使用 Header.Get 方法**

用于根据键访问头部的第一个值。这是最常用的方法，因为通常只有单个值与头部关联。下面是该方法的格式。

```go
func (h Header) Get(key string) string
```

请注意，返回类型只是一个字符串。它将返回与给定键关联的第一个值。此外，给定键在获取值之前会先转换为规范形式。可以如下调用。

```go
r.Header.Get(key)
```

例如

+   **r.Header.Get(“content-type”)**将输出**“application/json”**。

+   **r.Header.Get(“foo”)**将输出**“bar1”**。只会返回第一个值。

# **直接访问 Header 结构**

头部映射也可以直接访问，存在于***http.Request**中。

例如

+   **r.Header**将输出一个映射

```go
map[Accept:[*/*] Content-Type:[application/json] Foo:[bar1 bar2]
```

+   **r.Header[“Content-Type”]**将输出

```go
[application/json]
```

+   **r.Header[“Foo”]**将输出

```go
[bar1 bar2]
```

请注意，当直接访问头部映射时，我们需要仅以规范形式提供键。

# **示例**

让我们看看一个演示上述所有要点的程序。

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	handler := http.HandlerFunc(handleRequest)
	http.Handle("/example", handler)
	http.ListenAndServe(":8080", nil)
}

func handleRequest(w http.ResponseWriter, r *http.Request) {

	fooVAlues := r.Header.Values("foo")
	fmt.Printf("r.Header.Values(\"foo\"):: %s\n\n", fooVAlues)

	contentType := r.Header.Get("content-type")
	fmt.Printf("r.Header.Get(\"content-type\"):: %s\n\n", contentType)

	fooFirstValue := r.Header.Get("foo")
	fmt.Printf("r.Header.Get(\"foo\"):: %s\n\n", fooFirstValue)

	headers := r.Header
	fmt.Printf("r.Headers:: %s\n\n", headers)
	fmt.Printf("r.Headers[\"Content-Type\"]:: %s\n\n", headers["Content-Type"])
	fmt.Printf("r.Headers[\"Foo\"]:: %s", headers["Foo"])
}
```

在上述程序中，我们启动了一个监听在8080端口的服务器。我们还在该端点定义了一个URL。运行此服务器并进行以下API调用。

```go
curl -v -X POST http://localhost:8080/example -H "content-type: application/json" -H "foo: bar1" -H "foo: bar2"
```

运行此API后，请检查终端中的输出。它将输出。你可以检查输出，它与我们讨论的完全一致。

```go
r.Header.Values("foo"):: [bar1 bar2]

r.Header.Get("content-type"):: application/json

r.Header.Get("foo"):: bar1

r.Headers:: map[Accept:[*/*] Content-Type:[application/json] Foo:[bar1 bar2] User-Agent:[curl/7.54.0]]

r.Headers["Content-Type"]:: [application/json]

r.Headers["Foo"]:: [bar1 bar2]
```

+   [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/) *
