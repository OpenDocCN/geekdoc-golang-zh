<!--yml

分类: 未分类

日期: 2024-10-13 06:52:15

-->

# 在 Go (Golang) 中重复一个字符串多次

> 来源：[`golangbyexample.com/repeat-string-golang/`](https://golangbyexample.com/repeat-string-golang/)

目录

+   概述

+   程序

# **概述**

**strings.Repeat** 方法可用于在 Go (Golang) 中多次重复一个字符串

这是 Go 字符串包中该函数的链接

[`pkg.go.dev/strings#Repeat`](https://pkg.go.dev/strings#Repeat)

这是该方法的签名

```go
func Repeat(s string, count int) string
```

第一个参数是原始字符串，count 是字符串需要重复的次数

# **程序**

这是相同内容的程序

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	copy := strings.Repeat("a", 2)
	fmt.Println(copy)

	copy = strings.Repeat("abc", 3)
	fmt.Println(copy)
}
```

**输出:**

```go
aa
abcabcabc
```

**注意:** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们努力覆盖所有概念及示例。本教程适合那些希望获得专业知识和深入理解 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
