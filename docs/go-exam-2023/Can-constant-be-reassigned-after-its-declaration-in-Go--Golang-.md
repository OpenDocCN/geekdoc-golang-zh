<!--yml

分类：未分类

日期：2024-10-13 06:27:45

-->

# 在 Go (Golang) 中常量可以在声明后重新赋值吗

> 来源：[`golangbyexample.com/constant-reassign-go/`](https://golangbyexample.com/constant-reassign-go/)

目录

**   概述

+   示例*  *# **概述**

常量变量在声明后无法重新赋值，因为它是常量，且其值无法更改。如果尝试给常量变量重新赋值，则会引发编译错误。

# **示例**

例如，下面的代码将引发编译错误

```go
package main
func main() {
    const a int = 8
    a = 9
}
```

**输出**

```go
main.go:5:4: cannot assign to a
```

在上述程序中，我们首先创建了一个常量

```go
const a int = 8
```

然后我们尝试将新值 9 赋给常量**a**，因此会引发编译错误，因为常量一旦声明后无法重新赋值。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
