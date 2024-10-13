<!--yml

分类：未分类

日期：2024-10-13 06:26:01

-->

# 在 Go (Golang)中的协程中恢复 panic

> 来源：[`golangbyexample.com/recover-panic-goroutine-go/`](https://golangbyexample.com/recover-panic-goroutine-go/)

目录

+   概述

+   在相同协程中作为 panic 的 recover 函数

+   在不同协程中的 recover 函数作为 panic

# **概述**

恢复协程中恐慌的情况有两种。

+   **在相同协程中的 recover 函数作为**panic**

+   **在不同协程中的 recover 函数作为**panic**

在第一种情况下，它将从 panic 中恢复。但需要注意的一个重要点是，recover 函数只能恢复在同一协程中发生的 panic。如果 panic 发生在不同协程中，而 recover 也在不同协程中，则无法恢复。

让我们看看两个例子的情况。

# ****在相同协程中的 recover 函数作为 panic****

```go
package main

import "fmt"

func main() {
    a := []string{"a", "b"}
    go checkAndPrintWithRecover(a, 2)
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
Exiting normally
Recovering from panic: Out of bound access for slice
```

在上述程序中，**recover**和**panic**位于同一协程中，因此能够从 panic 中恢复，正如从输出中所见。

# **在不同协程中的 recover 函数作为 panic**

如前所述，在这种情况下无法从 panic 中恢复。

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
    go checkAndPrint(a, 2)
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
Exiting normally
panic: Out of bound access for slice

goroutine 18 [running]:
main.checkAndPrint(0xc0000a6020, 0x2, 0x2, 0x2)
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/goroutine/main.go:19 +0xe2
created by main.checkAndPrintWithRecover
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/goroutine/main.go:14 +0x82
exit status 2
```

在上述程序中，我们在协程中有**checkAndPrint**，它在该协程中引发了**panic**。**recover**函数在调用协程中。正如您从输出中看到的，它并没有停止**panic**，因此您看到了一些信息。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
