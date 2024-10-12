<!--yml
category: 未分类
date: 2024-10-13 06:34:50
-->

# Pause Execution of a goroutine until an activity or event is completed in Go (Golang)

> 来源：[https://golangbyexample.com/pause-goroutine/](https://golangbyexample.com/pause-goroutine/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Channels can be used to pause the execution of a goroutine until an activity is completed. For a

**Unbuffered Channel**

*   Goroutine will block on send operation if there is no other goroutine to receive

*   Goroutine will block on receive operation if there is no other goroutine to send on the other side

**Buffered Channel**

*   Goroutine will block on send operation if the buffer is full. 

*   Goroutine will block on receive operation if the buffer is empty

If we want a goroutine to pause until an activity is complete, we can use any of the above four ways. For simplicity in this tutorial, we are going to use an unbuffered channel. The goroutine will do a receive operation on that channel and it will pause. It will only resume if a send operation is done on that channel in a different goroutine. 

## **Program**

```
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

**Output**

```
Send Value: true
Received Value: true
```

In the above program, we create an unbuffered channel.  We start a goroutine which waits for a receive operation on that channel. It will be blocked until a send operation is done.

We then send the value in the main goroutine. Once the send is complete, then the goroutine resumes execution. That is why

```
Send Value: true
```

is always printed before

```
Send Value: true
Received Value: true
```

Also, check out our Golang comprehensive tutorial Series - [Golang Comprehensive Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*