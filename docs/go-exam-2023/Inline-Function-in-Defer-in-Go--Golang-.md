<!--yml

类别：未分类

日期：2024-10-13 06:26:47

-->

# Go（Golang）中的 defer 中的内联函数

> 来源：[`golangbyexample.com/inline-function-defer-go/`](https://golangbyexample.com/inline-function-defer-go/)

目录

**   概述

+   示例*  *# **概述**

也可以有一个带有 defer 的内联函数。

# **示例**

让我们看看一个示例。

```go
package main

import "fmt"

func main() {
    defer func() { fmt.Println("In inline defer") }()
    fmt.Println("Executed")
}
```

**输出**

```go
Executed
In inline defer
```

在上面的代码中，我们使用了一个内联函数的 defer。

```go
defer func() { fmt.Println("In inline defer") }()
```

这在 Go 中是允许的。还要注意，函数后面必须添加**“()”**，否则编译器会报错。

```go
expression in defer must be function call
```

从输出中可以看到，内联函数在主函数的所有内容执行完毕后被调用，并在主函数返回之前调用。这就是原因。

```go
Executed in main
```

在主函数执行之前打印了。

```go
In inline Defer
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
