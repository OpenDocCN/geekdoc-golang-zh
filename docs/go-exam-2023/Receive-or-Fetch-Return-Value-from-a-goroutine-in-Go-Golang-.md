<!--yml

分类：未分类

日期：2024-10-13 06:23:07

-->

# 从 Go（Golang）中的 goroutine 接收或获取返回值

> 来源：[`golangbyexample.com/return-value-goroutine-go/`](https://golangbyexample.com/return-value-goroutine-go/)

通道可以用来从 goroutine 获取返回值。通道提供了 goroutine 之间的同步和通信。你可以在 goroutine 中通过通道发送返回值，然后在 main 函数中收集该值。

让我们看看一个程序。

```go
package main

import "fmt"

func main() {
	result := make(chan int, 1)
	go sum(2, 3, result)

	value := <-result
	fmt.Printf("Value: %d\n", value)
	close(result)

}

func sum(a, b int, result chan int) {
	sumValue := a + b
	result <- sumValue
	return
}
```

**输出**

```go
Value: 5
```

在上面的程序中，我们创建了一个变量**result**，这是一个长度为 1、存放**int**类型值的通道。我们将这个通道传递给**sum**函数。**sum**函数将**sumValue**推送到**result**通道，如此操作。

```go
result <- sumValue
```

在 main 函数中，我们在等待**result**通道以收集结果。

```go
value := <-result
```

这一行将等待一个值被推送到**result**通道。如上所示，**sumValue**将通过**sum**函数推送到**result**通道。

为了说明这一行确实会等待，让我们在 sum 函数中设置一个超时，它实际上将**sumValue**推送到**result**通道。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	result := make(chan int, 1)
	go sum(2, 3, result)

	value := <-result
	fmt.Printf("Value: %d\n", value)
	close(result)

}

func sum(a, b int, result chan int) {
	sumValue := a + b
	time.Sleep(time.Second * 2)
	result <- sumValue
	return
}
```

**输出**

```go
Value: 5
```

程序输出相同，这证明**main**函数等待**sumValue**被推送到**result**通道。一旦从**result**通道接收到值，它就会打印出来。

+   [go](https://golangbyexample.com/tag/go/)  *   [golang](https://golangbyexample.com/tag/golang/)
