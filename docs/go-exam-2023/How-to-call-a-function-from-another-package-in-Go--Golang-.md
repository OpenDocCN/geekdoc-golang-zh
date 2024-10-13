<!--yml

类别：未分类

日期：2024-10-13 06:34:39

-->

# 如何在 Go (Golang)中从另一个包调用函数

> 来源：[`golangbyexample.com/functoin-different-package-go/`](https://golangbyexample.com/functoin-different-package-go/)

目录

+   概述

+   程序

## **概述**

另一个包中的函数必须以大写字母开头，以便在其包外部是公共的。如果函数名以小写字母开头，则在其包外部将不可见。

要在包外使用函数，我们需要先导入包含该函数的包。

## **程序**

这是相同的代码。

**go.mod**

```go
module sample.com/learn
go 1.16
```

**hello/hello.go**

```go
package hello

import "fmt"

func SayHello() {
	fmt.Println("Hello")
}
```

**main.go**

```go
package main

import "sample.com/learn/hello"

func main() {
    hello.SayHello()
}
```

**输出**

```go
Hello
```

在这个程序中，我们首先从**main**包导入**hello**包，如下所示。

```go
import "sample.com/learn/hello"
```

然后我们从**main**包调用**SayHello**函数，如下所示。

```go
hello.SayHello()
```

之所以有效，是因为**SayHello**函数是大写的。

将函数改为小写并运行此程序。它会产生以下编译错误。

```go
cannot refer to unexported name hello.sayHello
```

还可以查看我们的 Golang 综合教程系列 – [Golang 综合教程](https://golangbyexample.com/golang-comprehensive-tutorial/)


