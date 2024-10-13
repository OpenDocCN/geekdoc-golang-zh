<!--yml

类别：未分类

日期：2024-10-13 06:41:41

-->

# 在Go（Golang）中打印/输出下划线文本

> 来源：[https://golangbyexample.com/print-text-underline-golang/](https://golangbyexample.com/print-text-underline-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

我们可以使用faith包来实现相同的功能。

[https://github.com/fatih/color](https://github.com/fatih/color)

# **程序**

```
package main

import (
	"github.com/fatih/color"
)

func main() {
	whilte := color.New(color.FgWhite)
	boldWhite := whilte.Add(color.Underline)
	boldWhite.Println("This will print text in underline")
}
```

**输出**

![](img/2eb07d779bd64ce2bab39e19b5d1c89e.png)

**注意：** 请查看我们的Golang高级教程。本系列教程详细阐述了所有概念，并提供了示例。本教程适合那些希望获得专业知识和对golang有深入理解的人——[Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式在Golang中的实现方法。如果是的话，那么这篇文章就是为你准备的——[所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
