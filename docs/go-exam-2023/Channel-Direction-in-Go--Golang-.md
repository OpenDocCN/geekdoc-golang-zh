<!--yml

类别：未分类

日期：2024-10-13 06:24:33

-->

# Go（Golang）中的通道方向

> 来源：[`golangbyexample.com/channel-direction-go/`](https://golangbyexample.com/channel-direction-go/)

目录

+   概述

+   仅发送到通道

+   仅接收来自通道

# **概述**

在 golang 中可以创建双向和单向通道。可以创建一个只能发送数据的通道，也可以创建一个只能接收数据的通道。这由通道的箭头方向决定。通道的箭头方向指定数据流动的方向。

+   **chan**  :双向通道（同时读写）

+   **chan <-**  :仅向通道写入

+   **<- chan**  :仅从通道读取（输入通道）

**一个只能发送数据的通道。**

这是这样的通道的语法。

```go
chan<- int
```

**一个只能发送数据的通道**

这是这样的通道的语法。

```go
<-chan int
```

现在的问题是，为什么你想创建一个只能发送数据的通道或只能接收数据的通道。当我们希望将通道传递给一个函数，并限制该函数只能发送数据或接收数据时，这就很方便。

有很多方法可以将通道作为函数参数传递。

+   **chan**  :双向通道（同时读写）

+   **chan <-**  :仅向通道写入

+   **<- chan**  :仅从通道读取（输入通道）

# **仅发送到通道**

+   这样的通道只能发送数据，当作为参数传递给函数时，将如下所示。

```go
func process(ch chan<- int){ //doSomething }
```

+   尝试从这样的通道接收数据将会产生以下错误。

```go
invalid operation: <-ch (receive from send-only type chan<- int)
```

尝试取消注释下面的代码行以查看上述错误。

```go
s := <-ch
```

**代码：**

```go
package main
import "fmt"
func main() {
    ch := make(chan int, 3)
    process(ch)
    fmt.Println(<-ch)
}
func process(ch chan<- int) {
    ch <- 2
    //s := <-ch
}
```

**输出：** 2

# **仅接收来自通道**

+   这样的通道的签名，当作为参数传递给函数时，将如下所示。

```go
func process(ch <-chan int){ //doSomething }
```

+   尝试向这样的通道发送数据将会产生以下错误。

```go
invalid operation: ch <- 2 (send to receive-only type <-chan int)
```

尝试取消注释下面的代码行以查看上述错误。

```go
ch <- 2
```

**代码：**

```go
package main

import "fmt"

func main() {
    ch := make(chan int, 3)
    ch <- 2
    process(ch)
    fmt.Println()
}

func process(ch <-chan int) {
    s := <-ch
    fmt.Println(s)
    //ch <- 2
}
```

**输出：** 2


