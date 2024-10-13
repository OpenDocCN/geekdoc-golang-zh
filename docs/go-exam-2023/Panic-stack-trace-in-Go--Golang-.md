<!--yml

类别：未分类

日期：2024-10-13 06:26:27

-->

# Go（Golang）中的崩溃堆栈跟踪

> 来源：[`golangbyexample.com/panic-stack-trace-go/`](https://golangbyexample.com/panic-stack-trace-go/)

目录

+   概述

+   示例

# **概述**

golang 的**debug**包提供了一个**StackTrace**函数，可以用来打印 recover 函数中崩溃的堆栈跟踪。

# **示例**

让我们看看一个程序

```go
package main
import (
    "fmt"
    "runtime/debug"
)
func main() {
    a := []string{"a", "b"}
    checkAndPrint(a, 2)
    fmt.Println("Exiting normally")
}
func checkAndPrint(a []string, index int) {
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    fmt.Println(a[index])
}
func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
        fmt.Println("Stack Trace:")
        debug.PrintStack()
    }
}
```

**输出**

```go
Recovering from panic: Out of bound access for slice
Stack Trace:
goroutine 1 [running]:
runtime/debug.Stack(0xd, 0x0, 0x0)
        stack.go:24 +0x9d
runtime/debug.PrintStack()
        stack.go:16 +0x22
main.handleOutOfBounds()
        main.go:27 +0x10f
panic(0x10ab8c0, 0x10e8f60)
        /Users/slohia/Documents/goversion/go1.14.1/src/runtime/panic.go:967 +0x166
main.checkAndPrint(0xc000104f58, 0x2, 0x2, 0x2)
        main.go:18 +0x111
main.main()
        main.go:11 +0x81
Exiting normally
```

在上述程序中，我们有一个函数**checkAndPrint**，它检查并打印传入参数的索引处的切片元素。如果传入的索引大于数组的长度，程序将会出现崩溃。我们在函数**checkAndPrint**的开始处添加了一个名为**handleOutIfBounds**的延迟函数。这个函数包含了下面的 recover 函数调用。

```go
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

**recover**函数将捕获崩溃，我们也可以打印崩溃中的消息。

```go
Recovering from panic: Out of bound access for slice
```

我们使用**StackTrace**函数在 recover 函数中打印崩溃的堆栈跟踪。它打印出正确的堆栈跟踪，如输出所示。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
