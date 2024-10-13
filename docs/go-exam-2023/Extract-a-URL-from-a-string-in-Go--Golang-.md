<!--yml

类别：未分类

日期：2024-10-13 06:40:26

-->

# 在 Go（Golang）中从字符串中提取 URL

> 来源：[https://golangbyexample.com/extract-url-golang/](https://golangbyexample.com/extract-url-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

以下 Go 包可以用于从给定字符串中提取 URL

[https://github.com/mvdan/xurls](https://github.com/mvdan/xurls)

使用此包有两种方法

+   **严格** – 在严格模式下，它只匹配带有方案的 URL

+   **宽松** – 在宽松模式下，它匹配严格模式下的任何 URL，以及没有方案的任何 URL

你可以指定要过滤的方案。对此有一个函数

**StrictMatchingScheme**

# **程序**

让我们首先看一个程序

```go
package main

import (
	"fmt"

	"mvdan.cc/xurls/v2"
)

func main() {
	xurlsStrict := xurls.Strict()
	output := xurlsStrict.FindAllString("golangbyexample.com is https://golangbyexample.com", -1)
	fmt.Println(output)

	xurlsRelaxed := xurls.Relaxed()
	output = xurlsRelaxed.FindAllString("The website is golangbyexample.com", -1)
	fmt.Println(output)

	output = xurlsRelaxed.FindAllString("golangbyexample.com is https://golangbyexample.com", -1)
	fmt.Println(output)
}
```

**输出**

```go
[https://golangbyexample.com]
[golangbyexample.com]
[golangbyexample.com https://golangbyexample.com]
```

注意在严格模式下，它不会将 [golangbyexample.com](http://golangbyexample.com) 返回到输出中，因为该 URL 没有方案。

让我们看看另一个提取多个 URL 的程序

```go
package main

import (
	"fmt"

	"mvdan.cc/xurls/v2"
)

func main() {
	xurlsStrict := xurls.Strict()
	input := "The webiste is https://golangbyexample.com:8000/tutorials/intro amd mail to mailto:contactus@golangbyexample.com"
	output := xurlsStrict.FindAllString(input, -1)
	fmt.Println(output)
}
```

**输出**

```go
[https://golangbyexample.com:8000/tutorials/intro mailto:contactus@golangbyexample.com]
```

如果我们想将输出限制为特定方案，也可以做到。

```go
package main

import (
	"fmt"
	"log"

	"mvdan.cc/xurls/v2"
)

func main() {
	xurlsStrict, err := xurls.StrictMatchingScheme("https")
	if err != nil {
		log.Fatalf("Some error occured. Error: %s", err)
	}
	input := "The webiste is https://golangbyexample.com:8000/tutorials/intro amd mail to mailto:contactus@golangbyexample.com"
	output := xurlsStrict.FindAllString(input, -1)
	fmt.Println(output)
}
```

**输出**

```go
[https://golangbyexample.com:8000/tutorials/intro]
```

在上面的程序中，我们提供了 https 作为方案，这就是为什么我们只有一个输出

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
