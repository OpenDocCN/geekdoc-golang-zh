<!--yml

分类：未分类

日期：2024-10-13 06:23:38

-->

# Go（Golang）中的发送操作选择

> 来源：[`golangbyexample.com/select-send-operation-go/`](https://golangbyexample.com/select-send-operation-go/)

目录

**概述**

+   一次发送一次接收操作

+   所有发送操作

+   所有接收操作*  *# **概述**

Select 允许在其 case 语句中同时进行发送和接收操作。让我们看看以下示例：

+   一次发送一次接收操作

+   所有发送操作

+   所有接收操作

# **一次发送一次接收操作**

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go goOne(ch1)
    go goTwo(ch2)
    select {

    case msg1 := <-ch1:
        fmt.Println(msg1)
    case ch2 <- "To goTwo goroutine":
    }
}

func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}

func goTwo(ch chan string) {
    msg := <-ch
    fmt.Println(msg)
}
```

**输出**

```go
To goTwo goroutine
```

在上述程序中，我们创建了两个通道，并将其传递给两个不同的 goroutine。在 select 语句中，第一个 case 语句是从 **ch1** 通道接收数据。第二个 case 语句是向 **ch2** 通道发送数据，而这些数据在 **goTwo** goroutine 中被接收。由于无法确定 **ch1** 的接收操作是否会先完成，或 **ch2** 的发送操作是否会先完成，程序可能输出如下内容：

```go
To goTwo goroutine
```

或者这个

```go
From goOne goroutine
```

# **所有发送操作**

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go goOne(ch1)
    go goTwo(ch2)
    select {
    case ch1 <- "To goOne goroutine":
    case ch2 <- "To goTwo goroutine":
    }
    time.Sleep(time.Second * 1)
}

func goOne(ch chan string) {
    msg := <-ch
    fmt.Println(msg)
}    

func goTwo(ch chan string) {
    msg := <-ch
    fmt.Println(msg)
}
```

**输出**

```go
To goTwo goroutine
```

在上述程序中，两个 case 语句分别向 **ch1** 和 **ch2** 通道发送数据。来自 ch1 通道的数据在 goOne goroutine 中被接收，而来自 ch2 通道的数据在 goTwo goroutine 中被接收。每个 case 语句中的发送操作都没有被阻塞。因此，程序可能输出如下内容：

```go
To goOne goroutine
```

或者这个

```go
To goTwo goroutine
```

# **所有接收操作**

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go goOne(ch1)
    go goTwo(ch2)

    select {
    case msg1 := <-ch1:
        fmt.Println(msg1)
    case msg2 := <-ch2:
        fmt.Println(msg2)
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
```

在上述程序中，我们创建了两个通道，并将其传递给两个不同的 goroutine。然后，每个 goroutine 向通道发送一个值。在 select 中，我们有两个 case 语句。每个 case 语句都在等待一个通道上的接收操作完成。一旦任何通道上的接收操作完成，就会执行该操作并且 **select** 退出。因此，从输出中可以看出，在上述程序中，它打印了从一个通道接收到的值并退出。

因此，在上述程序中，由于无法确定哪个发送操作会先完成，所以如果你多次运行该程序，你将看到不同的输出。

+   [go](https://golangbyexample.com/tag/go/)*
