<!--yml

类别：未分类

日期：2024-10-13 06:34:50

-->

# 暂停Goroutine的执行，直到Go（Golang）中的活动或事件完成。

> 来源：[https://golangbyexample.com/pause-goroutine/](https://golangbyexample.com/pause-goroutine/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

通道可用于暂停Goroutine的执行，直到活动完成。对于一个

**无缓冲通道**

+   当没有其他Goroutine接收时，Goroutine将在发送操作上阻塞。

+   当没有其他Goroutine在另一端发送时，Goroutine将在接收操作上阻塞。

**缓冲通道**

+   当缓冲区满时，Goroutine将在发送操作上阻塞。

+   当缓冲区为空时，Goroutine将在接收操作上阻塞。

如果我们希望Goroutine暂停直到活动完成，我们可以使用上述四种方法中的任何一种。为了简化，本教程中我们将使用一个无缓冲通道。Goroutine将在该通道上进行接收操作并暂停。只有在另一个Goroutine在该通道上完成发送操作时，它才会恢复。

## **程序**

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan bool)
	go test(ch)

	time.Sleep(time.Second * 1)
	fmt.Printf("Send Value: %t\n", true)
	ch <- true

	time.Sleep((time.Second * 3))
}

func test(ch chan bool) {
	var value = <-ch
	fmt.Printf("Received Value: %t\n", value)
	fmt.Println("Doing some work")
}
```

**输出**

```go
Send Value: true
Received Value: true
```

在上述程序中，我们创建了一个无缓冲通道。我们启动一个Goroutine，等待该通道上的接收操作。它将在发送操作完成之前被阻塞。

然后我们在主Goroutine中发送值。一旦发送完成，Goroutine恢复执行。这就是为什么

```go
Send Value: true
```

始终在之前打印

```go
Send Value: true
Received Value: true
```

另外，请查看我们的Golang综合教程系列 - [Golang综合教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
