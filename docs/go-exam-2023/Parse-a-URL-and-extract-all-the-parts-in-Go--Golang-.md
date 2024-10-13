<!--yml

分类：未分类

日期：2024-10-13 06:40:21

-->

# 解析一个 URL 并提取所有部分，使用 Go (Golang)

> 来源：[`golangbyexample.com/parse-url-golang/`](https://golangbyexample.com/parse-url-golang/)

目录

**概述

+   程序

# **概述**

**net/url**包中包含一个 Parse 函数，可以用来解析给定的 URL 并返回 URL 结构的实例

[`golang.org/pkg/net/url/#URL`](https://golang.org/pkg/net/url/#URL)

一旦给定的 URL 被正确解析，它将返回 URI 对象。然后我们可以从 URI 访问以下信息

+   方案

+   用户信息

+   主机名

+   端口

+   路径名

+   查询参数

+   片段

让我们来看一个工作程序：

我们将解析以下 URL

```go
https://test:abcd123@golangbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

然后

+   方案是 HTTPS

+   用户信息 – 用户名是 test，密码是 abcd123。用户名和密码用冒号分隔：

+   主机名是 [www.golangbyexample.com](http://www.golangbyexample.com/)

+   端口是 8000

+   路径是 tutorials/intro

+   查询参数是**type=advance**和**compact=false**。它们用和号分隔

+   片段是**历史**。它将直接带你到该页面的历史部分。**历史**是一个标识符，指向该页面中的那个部分

# **程序**

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

	fmt.Println(u.Scheme)
	fmt.Println(u.User)
	fmt.Println(u.Hostname())
	fmt.Println(u.Port())
	fmt.Println(u.Path)
	fmt.Println(u.RawQuery)
	fmt.Println(u.Fragment)
	fmt.Println(u.String())
}
```

**输出**

```go
https
test:abcd123
golangbyexample.com
8000
/tutorials/intro
type=advance&compact=false
history
https://test:abcd123@golangbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

它正确地输出所有信息，如输出所示


