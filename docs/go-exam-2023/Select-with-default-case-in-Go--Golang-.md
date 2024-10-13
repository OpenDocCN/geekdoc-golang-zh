<!--yml

类别：未分类

日期：2024-10-13 06:23:33

-->

# 在 Go (Golang) 中的带有默认案例的 select

> 来源：[`golangbyexample.com/select-default-case-go/`](https://golangbyexample.com/select-default-case-go/)

目录

+   概述

+   代码

# **概述**

与 switch 类似，select 也可以有一个默认案例。如果在任何案例语句上没有 send 或 receive 操作准备好，则将执行这个默认案例。因此，默认语句在某种程度上防止了 select 永远阻塞。需要注意的一个非常重要的点是，默认语句使得 select 变为非阻塞的。如果 select 语句不包含默认案例，那么它可能会永远阻塞，直到某个案例语句上的 send 或 receive 操作准备好。让我们看看一个例子以便完全理解。

# **代码**

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    select {
    case msg := <-ch1:
        fmt.Println(msg)
    default:
        fmt.Println("Default statement executed")
    }
}
```

**输出**

```go
Default statement executed
```

在上面的程序中，有一个 select 语句正在等待 **ch1** 上的接收操作和一个默认语句。由于没有 goroutine 向通道 **ch1** 发送，因此执行了默认案例，然后 select 退出。

select 语句检查是否有值在任何案例语句的通道中可用。如果可用，则执行该案例，否则将立即执行默认案例。

让我们看一个例子，在这个例子中，值在其中一个通道中立即可用。我们会看到在这种情况下默认案例不会被执行。

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string, 1)
    ch1 <- "Some value"
    select {
    case msg := <-ch1:
        fmt.Println(msg)
    default:
        fmt.Println("Default statement executed")
    }
}
```

**输出**

```go
Some value
```

我们可以在上面的程序中看到，值在 ch1 通道上可用，这就是为什么执行了该案例而默认案例没有被执行。输出中也同样明显。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
