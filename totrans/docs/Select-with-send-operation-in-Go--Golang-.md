<!--yml
category: 未分类
date: 2024-10-13 06:23:38
-->

# Select with send operation in Go (Golang)

> 来源：[https://golangbyexample.com/select-send-operation-go/](https://golangbyexample.com/select-send-operation-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [One send one receive operation](#One_send_one_receive_operation "One send one receive operation")
*   [All send operations ](#All_send_operations "All send operations ")
*   [All receive operations](#All_receive_operations "All receive operations")*  *# **Overview**

Select allows both send and receive operations from a channel in its case statement. Let’s see examples of

*   One send one receive operation

*   All send operations 

*   All receive operations

# **One send one receive operation**

```
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

**Output**

```
To goTwo goroutine
```

In the above program we created two channels which are passed to two different goroutines. In the select statement, the first case statement is receiving from **ch1** channel. The second case statement is sending data to the **ch2** channel and that data is being received in the **goTwo** goroutine. Since it is not deterministic whether the receive operation on **ch1** will complete earlier or send operation on **ch2** will completely  earlier the program may either output this 

```
To goTwo goroutine
```

or this

```
From goOne goroutine
```

# **All send operations **

```
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

**Output**

```
To goTwo goroutine
```

In the above program, both case statements is sending data to the **ch1** and **ch2** channel respectively. The data from the ch1 channel is being received in goOne goroutine and the data from the ch2 channel is being received in goTwo goroutine. None of the send operation in each of the case statement is blocked.  So the program can either output this

```
To goOne goroutine
```

or this

```
To goTwo goroutine
```

# **All receive operations**

```
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

**Output**

```
From goOne goroutine
```

In the above program we created two channels which are passed to two different goroutines. Then each of the  goroutine  is sending one value to the channel. In the select  we have two case statement. Each of the two case statement is waiting for a receive operation to complete on one of the channels. Once any receive operation is complete on any of the channel it is executed and **select** exits. So as seen from output, in the above program it  prints the received value from one of the channel and exits.

So in the above program since it is not deterministic which of the send operation will complete earlier that is why you will see different output if you run the program different times.

*   [go](https://golangbyexample.com/tag/go/)*