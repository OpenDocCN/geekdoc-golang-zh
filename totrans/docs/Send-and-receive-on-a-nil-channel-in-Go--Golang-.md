<!--yml
category: 未分类
date: 2024-10-13 06:24:23
-->

# Send and receive on a nil channel in Go (Golang)

> 来源：[https://golangbyexample.com/send-receive-nil-channel-go/](https://golangbyexample.com/send-receive-nil-channel-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

The zero value of the channel is nil. Hence only declaring a channel creates a nil channel as default zero value of the channel is nil. Below is the result of send and receive operation on a nil channel

*   Sending to a  nil channel blocks forever

*   Receiving from nil channel blocks forever

Let’s see a program for it

# **Code**

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

**Output**

```
Sending value to channnel start
fatal error: all goroutines are asleep - deadlock!
goroutine 1 [chan receive (nil chan)]:
goroutine 18 [chan send (nil chan)]:
```

In the above program, we are only declaring the channel hence a nil Channel was created since the default 0 value of the channel is nil.  After that, we sent to the channel in send function and received from the channel in the main function.  It results in a deadlock as sending to and receiving from a nil channel block forever.  That is why it  gives below output

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*