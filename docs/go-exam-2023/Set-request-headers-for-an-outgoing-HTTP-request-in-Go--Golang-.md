<!--yml

category: 未分类

date: 2024-10-13 06:32:02

-->

# 在 Go (Golang) 中为输出的 HTTP 请求设置请求头部

> 来源：[`golangbyexample.com/set-headers-http-request/`](https://golangbyexample.com/set-headers-http-request/)

注意：相关帖子

+   从传入的 HTTP 请求中获取头部信息 – [`golangbyexample.com/headers-http-request-golang/`](https://golangbyexample.com/headers-http-request-golang/)

+   在 Go (Golang) 中为传入的 HTTP 请求设置响应头部 – [`golangbyexample.com/set-resposne-headers-http-go/`](https://golangbyexample.com/set-resposne-headers-http-go/)

+   获取 Golang 中的输出 HTTP 请求的响应头部 - [`golangbyexample.com/get-response-headers-making-go/`](https://golangbyexample.com/get-response-headers-making-go/)

现在让我们看看如何在进行 HTTP 请求时设置头部

目录

**   概述

+   使用 r.Header.Add() 方法 方法")

+   使用 r.Header.Set 方法

+   示例*  *# **概述**

以下是头部在 Go 中表示的格式。

```go
type Header map[string][]string
```

所以头部是一个键值对

+   键以规范形式表示。规范形式意味着第一个字符和任何在连字符后的字符都是大写。所有其他字符为小写。规范形式的示例为

```go
Content-Type
Accept-Encoding
```

+   值表示为字符串切片。为什么使用字符串数组？因为在请求中用相同键和不同值的两个头部是完全可以的。两个值将被收集到切片中。

例如，如果在客户端的输出请求中设置了以下头部，则

```go
user-agent: goapplication
foo: bar1
foo: bar2
```

然后在服务器上，头部将如下所示

```go
User-Agent: goapplication
Foo: bar1
Foo: bar2
```

请注意：

+   **user-agent** 被转换为规范形式 **User-Agent**

+   **foo** 头部被转换为 **Foo**。还要注意，将在请求中设置两个具有相同键 **foo** 的头部。其值将是 **bar1** 和 **bar2**

现在我们已经看到头部在请求中的表示。让我们看看在进行 HTTP 请求时如何设置请求头部值。假设我们有以下需要在 HTTP 请求中设置的键值对头部

```go
user-agent: goapplication
foo: bar1
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

在下面的示例中，假设变量 **r** 的类型为 ***http.Request**。现在让我们看看设置头部的不同方式

# **使用 r.Header.Add() 方法**

以下是 Add 方法的签名

```go
func (h Header) Add(key, value string)
```

此方法用于将键值对添加到请求头部。我们已经看到，头部值也可以是数组。因此，此方法将附加到可能已与该键关联的现有值上。此外，键将被转换为规范形式。例如，如果我们用不同的值两次添加 foo 头部

```go
r.Header.Add("foo", "bar1")
r.Header.Add("foo", "bar2")
```

然后**foo**头将被设置为**[“bar1”, “bar2”]**。同时，**foo**在进行 HTTP 调用之前将变为**Foo**

# **使用 r.Header.Set 方法**

以下是函数的签名

```go
func (h Header) Set(key, value string)
```

它可用于设置与给定键相关联的头部条目。与**Add**方法不同，此方法会替换与该键相关联的任何现有值。同时，键将被转换为规范形式。

# **示例**

让我们看看一个程序，阐明以上所有要点

```go
package main
import (
    "fmt"
    "net/http"
    "time"
)
func main() {
    call("https://google.com", "GET")
}
func call(url, method string) error {
    client := &http.Client{
        Timeout: time.Second * 10,
    }
    req, err := http.NewRequest(method, url, nil)
    if err != nil {
        return fmt.Errorf("Got error %s", err.Error())
    }
    req.Header.Set("user-agent", "golang application")
    req.Header.Add("foo", "bar1")
    req.Header.Add("foo", "bar2")
    response, err := client.Do(req)
    if err != nil {
        return fmt.Errorf("Got error %s", err.Error())
    }
    defer response.Body.Close()
    return nil
}
```

请注意在上述示例中我们在进行 HTTP 调用时如何设置头部

```go
req.Header.Set("user-agent", "goapplication")
req.Header.Add("foo", "bar1")
req.Header.Add("foo", "bar2")
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
