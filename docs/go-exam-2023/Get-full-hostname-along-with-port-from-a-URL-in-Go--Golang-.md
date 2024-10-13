<!--yml

类别：未分类

日期：2024-10-13 06:40:37

-->

# 在Go（Golang）中获取URL的完整主机名及端口。

> 来源：[https://golangbyexample.com/hostname-port-url-golang/](https://golangbyexample.com/hostname-port-url-golang/)

目录

**   [概览](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概览**

**golang的net/url包**包含一个解析函数，可以用来解析给定的URL并返回URL结构体的实例。[https://golang.org/pkg/net/url/#URL](https://golang.org/pkg/net/url/#URL)

一旦给定的URL正确解析，将返回URI对象。我们可以从URI中访问以下信息。

+   协议

+   用户信息

+   主机名

+   端口

+   路径名

+   查询参数

+   片段

一旦我们拥有所有部分，就可以将它们连接起来，以获取完整的主机名及端口。我们将解析以下URL。

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
