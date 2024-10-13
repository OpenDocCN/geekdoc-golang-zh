<!--yml

类别：未分类

日期：2024-10-13 06:24:37

-->

# Go（Golang）中通道的长度和容量

> 来源：[https://golangbyexample.com/length-and-capacity-channel-golang/](https://golangbyexample.com/length-and-capacity-channel-golang/)

目录

**   [概述](#Overview "Overview")

+   [缓冲通道的长度和容量](#Length_and_capacity_of_a_buffered_channel "Length and capacity of a buffered channel")

+   [无缓冲通道的长度和容量](#Length_and_capacity_of_a_unbuffered_channel "Length and capacity of a unbuffered channel")

+   [Nil通道的长度和容量](#Length_and_capacity_of_nil_channel "Length and capacity of nil channel")*  *# **概述**

仅缓冲通道适用长度和容量。通道的长度是通道中已有元素的数量，而缓冲通道的容量是该通道可以容纳的元素数量。因此，长度实际上表示通道缓冲区中排队的元素数量，而容量则指通道缓冲区的大小。因此，通道的长度始终小于或等于通道的容量。

+   可以使用len()函数获取通道的长度

+   可以使用cap()函数获取通道的容量

无缓冲通道的长度和容量始终为零

# **缓冲通道的长度和容量**

```go
package main

import "fmt"

func main() {
	ch := make(chan int, 3)
	ch <- 5
	fmt.Printf("Len: %d\n", len(ch))
	fmt.Printf("Capacity: %d\n", cap(ch))

	ch <- 6
	fmt.Printf("Len: %d\n", len(ch))
	fmt.Printf("Capacity: %d\n", cap(ch))

	ch <- 7
	fmt.Printf("Len: %d\n", len(ch))
	fmt.Printf("Capacity: %d\n", cap(ch))
}
```

**输出**

```go
Len: 1
Capacity: 3
Len: 2
Capacity: 3
Len: 3
Capacity: 3
```

在上面的代码中，首先创建了一个容量为3的通道。之后，我们不断向通道发送一些值。正如你从输出中注意到的那样，每次发送操作后通道的长度增加1，而容量始终保持为3。

# **无缓冲通道的长度和容量**

无缓冲通道的长度和容量始终为零

```go
package main

import "fmt"

func main() {
    ch := make(chan int)
    fmt.Printf("Len: %d\n", len(ch))
    fmt.Printf("Capacity: %d\n", cap(ch))
}
```

**输出**

```go
Len: 0
Capacity: 0
```

# **Nil通道的长度和容量**

Nil通道的长度和容量始终为零

```go
package main

import "fmt"

func main() {
	var ch chan int

	fmt.Printf("Len: %d\n", len(ch))
	fmt.Printf("Capacity: %d\n", cap(ch))
}
```

**输出**

```go
Len: 0
Capacity: 0
```

以下是不同类型通道上**len()**和**cap()**的结果摘要表

| **命令** | **无缓冲通道**（未关闭且不为nil） | **缓冲通道**（未关闭且不为nil） | **已关闭通道** | **Nil通道** |
| --- | --- | --- | --- | --- |
| 长度 | 0 | 通道缓冲区中排队的元素数量 | -如果是无缓冲通道则为0-如果是缓冲通道则为缓冲区中排队的元素数量 | 0 |
| 容量 | 0 | 通道缓冲区的大小 | -如果是无缓冲通道则为0-如果是缓冲通道则为缓冲区的大小 | 0 |

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
