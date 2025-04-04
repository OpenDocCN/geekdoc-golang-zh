<!--yml

类别：未分类

日期：2024-10-13 06:52:39

-->

# 在 Go (Golang)中创建或初始化一个新字符串

> 来源：[`golangbyexample.com/create-string-golang/`](https://golangbyexample.com/create-string-golang/)

目录

**   概览

+   程序

# **概览**

下面是一种简单的方法来初始化或创建一个 Go 中的字符串。在下面的程序中，我们简单地声明并定义了一个名为**sample**的字符串。

注意语法

```go
sample := "test"
```

这是完整的程序

# 程序

```go
package main

import "fmt"

func main() {
	sample := "test"
	fmt.Println(sample)
}
```

**输出**

```go
test
```

如果我们只想声明一个字符串变量，下面就是方法。声明的字符串会初始化为空字符串。

```go
package main

import "fmt"

func main() {
    var sample string
    fmt.Println(sample)
}
```

**输出**

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尝试覆盖所有概念及示例。此教程适合希望获得专业知识和扎实理解 Golang 的人士 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是，那么这篇文章就是为你准备的 –

[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

**注意：** 请查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)*
