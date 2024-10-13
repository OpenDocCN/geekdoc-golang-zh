<!--yml

类别：未分类

日期：2024-10-13 06:27:19

-->

# Go (Golang)中的 Defer 函数和命名返回值

> 来源：[`golangbyexample.com/defer-named-return-values-golang/`](https://golangbyexample.com/defer-named-return-values-golang/)

目录

+   概述

+   示例

# **概述**

在函数中如果有命名返回值，defer 函数可以读取和修改这些命名返回值。如果 defer 函数修改了命名返回值，则该修改后的值将被返回。

让我们看看一个程序

# **示例**

```go
package main
import "fmt"
func main() {
    s := test()
    fmt.Println(s)
}
func test() (size int) {
    defer func() { size = 20 }()
    size = 30
    return
}
```

**输出**

```go
20
```

在上面的程序中，我们在测试函数中命名返回值为**“size”**。在 defer 函数中，我们修改了命名返回值，并将值更改为 20。然后我们将 size 设置为 30。在主函数中，我们打印测试函数的返回值，输出为 20 而不是 30，因为 defer 函数已修改测试函数中 size 变量的值。


