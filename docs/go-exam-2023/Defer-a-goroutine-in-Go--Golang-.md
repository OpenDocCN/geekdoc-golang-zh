<!--yml

分类：未分类

日期：2024-10-13 06:25:51

-->

# 在 Go (Golang) 中延迟一个 goroutine

> 来源：[`golangbyexample.com/defer-goroutine-golang/`](https://golangbyexample.com/defer-goroutine-golang/)

目录

+   概述

+   示例

# **概述**

直接延迟一个 goroutine 是不可能的。但有一种变通方法。在 defer 函数中，你可以像下面这样在 goroutine 中调用另一个函数

```go
defer func() {
        go some_function()
}()
```

# **示例**

让我们看看一个程序

```go
package main
import (
    "fmt"
    "time"
)
func main() {
    call()
    time.Sleep(time.Second * 2)
}
func call() {
    defer func() {
        go test()
    }()
    fmt.Println("In call function")
}
func test() {
    fmt.Println("In test function")
}
```

**输出**

```go
In call function
In test function
```

查看我们如何在 **调用** 函数中间接延迟一个 goroutine

```go
defer func() {
        go test()
}()
```


