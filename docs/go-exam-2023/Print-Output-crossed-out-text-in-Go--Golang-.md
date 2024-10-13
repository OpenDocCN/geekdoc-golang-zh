<!--yml

类别：未分类

日期：2024-10-13 06:41:36

-->

# 在 Go（Golang）中打印/输出删除线文本

> 来源：[`golangbyexample.com/print-text-crossout-golang/`](https://golangbyexample.com/print-text-crossout-golang/)

目录

+   概述

+   程序

# **概述**

我们可以使用 faith 包来实现相同的功能

[`github.com/fatih/color`](https://github.com/fatih/color)

# **程序**

```go
package main

import (
	"github.com/fatih/color"
)

func main() {
	whilte := color.New(color.FgWhite)
	boldWhite := whilte.Add(color.CrossedOut)
	boldWhite.Println("This will print text in crossout")
}
```

**输出**

![](img/12f7cb1df58a4a34263e258c0b035cd1.png)

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，涵盖了所有概念和实例。此教程适合那些希望获得 Golang 专业知识和扎实理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章就是为你准备的 –[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


