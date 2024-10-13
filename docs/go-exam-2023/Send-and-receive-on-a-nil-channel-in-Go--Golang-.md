<!--yml

类别：未分类

日期：2024-10-13 06:24:23

-->

# 在Go（Golang）中在空通道上发送和接收

> 来源：[https://golangbyexample.com/send-receive-nil-channel-go/](https://golangbyexample.com/send-receive-nil-channel-go/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# **概述**

通道的零值是nil。因此，仅声明一个通道会创建一个空通道，因为通道的默认零值是nil。以下是对空通道的发送和接收操作的结果

+   向空通道发送会永远阻塞

+   从空通道接收会永远阻塞

让我们看看一个程序

# **代码**

```
package main

import (
	"fmt"
	"time"
)

func main() {
	var ch chan int

	go send(ch)
	<-ch
	time.Sleep(time.Second * 1)
}

func send(ch chan int) {
	fmt.Println("Sending value to channnel start")
	ch <- 1
	fmt.Println("Sending value to channnel finish")
}
```

**输出**

```
Sending value to channnel start
fatal error: all goroutines are asleep - deadlock!
goroutine 1 [chan receive (nil chan)]:
goroutine 18 [chan send (nil chan)]:
```

在上面的程序中，我们只是声明了通道，因此创建了一个空通道，因为通道的默认值是零，也就是nil。之后，我们在发送函数中向通道发送数据，并在主函数中从通道接收。这导致了死锁，因为向空通道发送和接收会永远阻塞。因此，它给出的输出如下

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
