<!--yml

类别：未分类

日期：2024-10-13 06:23:53

-->

# Go（Golang）中的选择语句中的 Fallthrough 关键字

> 来源：[`golangbyexample.com/fallthrough-keyword-select-golang/`](https://golangbyexample.com/fallthrough-keyword-select-golang/)

目录

+   概述

+   代码

# **概述**

选择语句不允许**fallthrough**关键字选择多个案例。**Fallthrough**关键字只能在 switch 语句中用于选择多个案例。在准备好的案例中只会随机选择一个案例。它无法执行多个案例，但有一个解决方法。我们可以在 set 语句外部有一个 for 循环。这个 for 循环将根据循环的迭代次数调用选择语句。

让我们看看一个例子。

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

在上面的程序中，我们在选择语句中放置了一个长度为二的 for 循环。因此，选择语句执行了两次，并打印了每个 case 语句接收到的值。

通过这种方式，我们可以执行多个 case 语句，但请注意，这并不是确定性的。如果在某个特定 case 语句上有多个操作可用，则该 case 在 for 循环的所有迭代中每次都可能被执行。


