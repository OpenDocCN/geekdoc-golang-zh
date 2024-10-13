<!--yml

类别：未分类

日期：2024-10-13 06:24:03

-->

# 在 Go (Golang) 的 Select 语句中执行多个案例

> 来源：[`golangbyexample.com/execute-multiple-case-select-go/`](https://golangbyexample.com/execute-multiple-case-select-go/)

目录

+   概述

+   代码

# **概述**

select 语句仅在发送或接收通道操作准备好的情况下执行其中一个案例。它不能执行多个案例，但有一个变通办法。我们可以在 set 语句外部放一个 for 循环。这个 for 循环会调用 select 语句，次数等于循环的迭代次数。

让我们看一个例子。

# **代码**

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go goOne(ch1)
    go goTwo(ch2)
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        }
    }
}

func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}

func goTwo(ch chan string) {
    ch <- "From goTwo goroutine"
}
```

**输出**

```go
From goOne goroutine
From goTwo goroutine
```

在上面的程序中，我们在 select 语句上放置了一个长度为二的 for 循环。因此，select 语句执行了两次，并打印出每个案例语句接收到的值。

这样我们可以执行多个案例语句，但请注意这并不是确定性的。如果对于特定的案例语句，在同一通道上有多个操作可用，那么该案例在所有迭代中都可以被执行。

+   [go](https://golangbyexample.com/tag/go/)*
