<!--yml

类别：未分类

日期：2024-10-13 06:26:58

-->

# Go (Golang) 中的 defer 中的自定义函数

> 来源：[`golangbyexample.com/custom-function-defer-golang/`](https://golangbyexample.com/custom-function-defer-golang/)

目录

+   概述

+   示例

# **概述**

我们也可以在 defer 中调用自定义函数。让我们来看一个示例

# **示例**

```go
package main
import "fmt"
func main() {
    defer test()
    fmt.Println("Executed in main")
}
func test() {
    fmt.Println("In Defer")
}
```

**输出**

```go
Executed in main
In Defer
```

在上述程序中，有一个**defer**语句调用了名为**test**的自定义函数。从输出可以看到，**test**函数在主函数中的所有操作执行完毕后被调用，并在主函数返回之前调用。这就是为什么

```go
Executed in main
```

会在之前打印

```go
In Defer
```

上述函数还显示在主函数中使用 defer 是完全可以的。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
