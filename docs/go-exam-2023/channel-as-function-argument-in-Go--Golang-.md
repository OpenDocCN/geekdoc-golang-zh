<!--yml

类别: 未分类

日期: 2024-10-13 06:08:53

-->

# 在Go语言中将通道作为函数参数

> 来源：[https://golangbyexample.com/channel-function-argument-go/](https://golangbyexample.com/channel-function-argument-go/)

目录

**   [概述](#Overview "概述")

+   [双向通道](#Bidirectional_channel "双向通道")

+   [仅发送通道](#Only_Send_Channel "仅发送通道")

+   [仅接收通道](#Only_Receive_Channel "仅接收通道")

+   [通道指针](#Channel_Pointer "通道指针")*  *# 概述

通道可以作为函数参数传递的方式有很多。箭头的方向指定数据流的方向

+   **chan** : 双向通道（可读可写）

+   **chan <-** : 仅写入通道

+   **<- chan** : 仅从通道读取（输入通道）

+   ***chan** : 通道指针。可读可写

# **双向通道**

这样的双向通道的签名在作为函数参数传递时如下所示。

```go
func process(ch chan int){ //doSomething }
```

**代码:**

```go
package main
import "fmt"
func main() {
    ch := make(chan int, 3)
    process(ch)
}
func process(ch chan int) {
    ch <- 2
    s := <-ch
    fmt.Println(s)
}
```

**输出:** 2

# **仅发送通道**

+   只能发送数据的通道的签名在作为函数参数传递时如下所示。

```go
func process(ch chan<- int){ //doSomething }
```

+   尝试从这样的通道接收数据时会出现以下错误。

```go
invalid operation: <-ch (receive from send-only type chan<- int)
```

尝试取消注释代码中的以下行以查看上述错误

```go
s := <-ch
```

**代码:**

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

**输出:** 2

# **仅接收通道**

+   仅接收数据的通道的签名在作为函数参数传递时如下所示

```go
func process(ch <-chan int){ //doSomething }
```

+   尝试向这样的通道发送数据时会出现以下错误。

```go
invalid operation: ch <- 2 (send to receive-only type <-chan int)
```

尝试取消注释代码中的以下行以查看上述错误

```go
ch <- 2
```

**代码:**

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

**输出:** 2

# **通道指针**

以这种方式传递通道只有在您想要修改通道时才有意义。这是非常不常见的，且不是一种可取的使用方式。这样的通道作为指针传递的签名。

```go
func process(ch *chan int){ //doSomething}
```

**代码:**

```go
package main

import "fmt"

func main() {
	ch := make(chan int, 3)
	process(&ch)
	fmt.Println(ch)
}

func process(ch *chan int) {
	*ch <- 2
	s := <-*ch
	*ch = nil
	fmt.Println(s)
} 
```

**输出:** 2*
