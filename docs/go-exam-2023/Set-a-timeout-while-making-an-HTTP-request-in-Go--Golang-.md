<!--yml

类别：未分类

日期：2024-10-13 06:33:51

-->

# 在 Go（Golang）中发起 HTTP 请求时设置超时

> 来源：[`golangbyexample.com/set-timeout-http-golang/`](https://golangbyexample.com/set-timeout-http-golang/)

目录

+   概述

+   程序

# **概述**

**client** 结构体来自 HTTP 包，可以用来指定超时。在创建**HTTP**客户端时，我们可以指定超时的值。关于 HTTP 客户端需要注意的一点是，它只会被创建一次，并且同一个实例会被用于多个 HTTP 请求。以下是**http.Client**结构体的结构。

```go
type Client struct {
	Transport RoundTripper
	CheckRedirect func(req *Request, via []*Request) error
	Jar CookieJar
	Timeout time.Duration
}
```

正如你所看到的，可以为**http.Client**指定超时选项，即**Timeout**字段。

现在让我们看一个工作示例。

# **程序**

在下面的程序中，我们设置了 1 纳秒的超时，以展示在发起请求时确实会发生超时。

```go
package main
import (
    "fmt"
    "net/http"
    "os"
    "time"
)
func main() {
    client := &http.Client{
        Timeout: time.Nanosecond * 1,
    }
    _, err := client.Get("https://google.com")
    if os.IsTimeout(err) {
        fmt.Println("Timeout Happened")
    } else {
        fmt.Println("Timeout Did not Happened")
    }
}
```

**输出**

```go
Timeout Happened
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
