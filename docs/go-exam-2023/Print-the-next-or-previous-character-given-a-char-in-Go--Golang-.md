<!--yml

类别：未分类

日期：2024-10-13 06:52:10

-->

# 在Go语言中打印给定字符的下一个或上一个字符。

> 来源：[https://golangbyexample.com/next-previous-char-golang/](https://golangbyexample.com/next-previous-char-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

通过对当前字符进行加1和减1，我们可以得到下一个和上一个字符。

# **程序**

这里是相应的程序。

```
package main

import "fmt"

func main() {
	var curr byte
	curr = 'b'

	fmt.Println(string(curr + 1))
	fmt.Println(string(curr - 1))
} 
```

**输出：**

```
c
a
```

**注意：** 请查看我们的Golang高级教程。本系列教程内容丰富，我们努力涵盖所有概念并附有示例。这个教程适合那些希望获得专业知识和扎实理解Golang的人 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章就是为你准备的 – [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*
