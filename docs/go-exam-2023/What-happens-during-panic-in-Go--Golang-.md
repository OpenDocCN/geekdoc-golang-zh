<!--yml

类别: 未分类

日期: 2024-10-13 06:27:08

-->

# 在 Go（Golang）中发生恐慌时会发生什么

> 来源：[`golangbyexample.com/what-happens-during-panic-go/`](https://golangbyexample.com/what-happens-during-panic-go/)

目录

+   概述

+   程序

# **概述**

让我们了解当程序发生恐慌时会发生什么。想象一个从**main**函数调用到**f1**函数再到**f2**函数的调用

**main**->**f1**->**f2**

现在假设在函数**f2**中发生了恐慌，以下是将发生的事件顺序

+   执行**f2**将会停止。如果**f2**中存在延迟函数，则会执行这些延迟函数。控制将返回给调用者，也就是函数**f1**。

+   **f1**函数的行为将类似于在该函数中发生恐慌，之后调用将返回给调用者，即**main**函数。请注意，如果中间还有更多函数，则该过程将以类似方式向上延续堆栈

+   **main**函数的行为也将像是在该函数中发生了恐慌，之后程序将崩溃

+   一旦程序崩溃，将打印恐慌消息以及此堆栈跟踪

# **程序**

让我们看看一个程序

```go
package main
import "fmt"
func main() {
    f1()
}
func f1() {
    defer fmt.Println("Defer in f1")
    f2()
    fmt.Println("After painc in f1")
}
func f2() {
    defer fmt.Println("Defer in f2")
    panic("Panic Demo")
    fmt.Println("After painc in f2")
}
```

**输出**

```go
Defer in f2
Defer in f1
panic: Panic Demo

goroutine 1 [running]:
main.f2()
        main.go:17 +0x95
main.f1()
        main.go:11 +0x96
main.main()
        main.go:6 +0x20
exit status 2
```

在上述程序中，恐慌发生在**f2**函数中，如下所示

```go
panic("Panic Demo")
```

**f2**中的延迟函数在那之后被调用，并打印以下消息

```go
Defer in f2
```

请注意，恐慌发生在**f2**函数中时，其执行停止，因此下面的代码行如果**f2**将不会被执行

```go
fmt.Println("After painc in f2")
```

控制返回到**f1**，如果它有一个延迟函数，延迟函数将被执行，并打印以下消息

```go
Defer in f1
```

之后控制返回到主函数，然后程序崩溃。输出打印恐慌消息以及从主函数到**f1**再到**f2**的整个堆栈跟踪。


