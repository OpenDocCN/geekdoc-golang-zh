<!--yml

分类：未分类

日期：2024-10-13 06:40:42

-->

# 在Go（Golang）中获取或提取URL的查询参数。

> 来源：[https://golangbyexample.com/query-params-url-golang/](https://golangbyexample.com/query-params-url-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

URL实例的查询函数可用于获取URL中存在的查询参数。

[https://pkg.go.dev/net/url#URL.Query](https://pkg.go.dev/net/url#URL.Query)

请注意，在Golang中，查询参数表示如下。

[https://pkg.go.dev/net/url#Values](https://pkg.go.dev/net/url#Values)

这是

```
map[string][]string
```

这实际上意味着特定的查询参数键值可以有一个或多个值。

示例

```
http://localhost:8080/products?filters=color&filters=price&order=asc
```

在此URL中，**filters**键有两个值——**color**和**price**，而**order**键只有一个值**asc**。

# **程序**

让我们来看一个工作示例。

```
package main

import (
	"fmt"
	"log"
	"net/url"
)

func main() {
	input_url := "http://localhost:8080/products?filters=color&filters=price&order=asc"
	u, err := url.Parse(input_url)
	if err != nil {
		log.Fatal(err)
	}

	queryParams := u.Query()

	fmt.Println(queryParams)

	fmt.Println(queryParams["filters"])

	fmt.Println(queryParams["order"])

}
```

**输出**

```
map[filters:[color price] order:[asc]]
[color price]
[asc]
```

**注意：** 请查看我们的Golang高级教程。本系列教程内容详尽，我们努力涵盖所有概念及示例。本教程适合希望获得Golang专业知识和扎实理解的人——[Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在Golang中实现所有设计模式。如果是，那么这篇文章适合你——[所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
