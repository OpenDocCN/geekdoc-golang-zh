<!--yml

类别：未分类

日期：2024-10-13 06:40:37

-->

# 在 Go（Golang）中获取 URL 的完整主机名及端口。

> 来源：[`golangbyexample.com/hostname-port-url-golang/`](https://golangbyexample.com/hostname-port-url-golang/)

目录

**   概览

+   程序*  *# **概览**

**golang 的 net/url 包**包含一个解析函数，可以用来解析给定的 URL 并返回 URL 结构体的实例。[`golang.org/pkg/net/url/#URL`](https://golang.org/pkg/net/url/#URL)

一旦给定的 URL 正确解析，将返回 URI 对象。我们可以从 URI 中访问以下信息。

+   协议

+   用户信息

+   主机名

+   端口

+   路径名

+   查询参数

+   片段

一旦我们拥有所有部分，就可以将它们连接起来，以获取完整的主机名及端口。我们将解析以下 URL。

```go
https://test:abcd123@golangbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

然后完整的主机名及端口将为

```go
https://golangbyexample.com:8000
```

# **程序**

以下是相应的程序。

```go
package main

import (
	"fmt"
	"log"
	"net/url"
)

func main() {
	input_url := "https://test:abcd123@golangbyexample.com:8000/tutorials/intro?type=advance&compact=false#history"
	u, err := url.Parse(input_url)
	if err != nil {
		log.Fatal(err)
	}

	fullHostname := u.Scheme + "://" + u.Host

	fmt.Println(fullHostname)
}
```

**输出**

```go
https://golangbyexample.com:8000:8000
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
