<!--yml

类别：未分类

日期：2024-10-13 06:25:30

-->

# 在 Go 语言中在不同函数中恢复 panic

> 来源：[`golangbyexample.com/recover-panic-different-function-go/`](https://golangbyexample.com/recover-panic-different-function-go/)

目录

**   概述

+   程序*  *# **概述**

如果**defer**函数和**recover**函数没有从引发恐慌的函数中被调用，那么在这种情况下**panic**也可以在被调用的函数中恢复。实际上，panic 可以在调用栈的后续链中恢复，让我们来看一个例子。

# **程序**

```go
package main

import "fmt"

func main() {
    a := []string{"a", "b"}
    checkAndPrintWithRecover(a, 2)
    fmt.Println("Exiting normally")
}

func checkAndPrintWithRecover(a []string, index int) {
    defer handleOutOfBounds()
    checkAndPrint(a, 2)
}

func checkAndPrint(a []string, index int) {
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    fmt.Println(a[index])
}

func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
    }
}
```

**输出**

```go
Recovering from panic: Out of bound access for slice
Exiting normally
```

在上面的程序中，我们有一个函数**checkAndPrint**，它检查并打印传入参数的索引处的切片元素。如果传入的索引大于数组的长度，程序将会引发 panic。

我们还有另一个函数**checkAndPrintWithRecover**，它包含对

我们还有另一个函数**checkAndPrintWithRecover**，它包含对

+   带有**recover**的**defer**函数，名为**handleOutOfBounds**

+   调用**checkAndPrint**函数

所以我们在函数**checkAndPrintWithRecover**的开头有一个名为**handleOutIfBounds**的延迟函数。这个函数包含如下的 recover 函数调用。

```go
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

**checkAndPrint**函数引发了 panic，但没有 recover 函数，相反，recover 的调用在**checkAndPrintWithRecover**函数中。我们将索引 2 传递给**checkAndPrint**函数，这个索引超出了边界。因此**checkAndPrint**引发了 panic，但程序仍然能够从 panic 中恢复，正如输出所示。这是因为 panic 可以在被调用的函数中恢复，并且在调用链中也可以恢复。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
