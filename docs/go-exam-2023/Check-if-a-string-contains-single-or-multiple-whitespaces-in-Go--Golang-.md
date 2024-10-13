<!--yml

类别：未分类

日期：2024-10-13 06:52:44

-->

# 检查字符串在 Go (Golang) 中是否包含单个或多个空格

> 来源：[`golangbyexample.com/string-whitespace-golang/`](https://golangbyexample.com/string-whitespace-golang/)

目录

+   概述

+   程序

# **概述**

可以使用简单的正则表达式来检查字符串在 Go 中是否包含单个或多个空格。

这是相同的程序

# **程序**

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	//Single whitespace
	sampleWord := "Good morning"

	isWhitespacePresent := regexp.MustCompile(`\s`).MatchString(sampleWord)
	fmt.Println(isWhitespacePresent)

	//Multiple Whitespace
	sampleWord = "Good   morning"
	isMultipleWhitespacePresent := regexp.MustCompile(`\s*`).MatchString(sampleWord)
	fmt.Println(isMultipleWhitespacePresent)
}
```

**输出**

```go
true
true
```

**注意：** 请查看我们的 Golang 高级教程。该系列教程内容详尽，我们尝试用例子覆盖所有概念。本教程适合那些希望获得专业知识和对 Golang 有深入理解的人— [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，那么这篇文章就是为你准备的—

[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

**注意：** 请查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
