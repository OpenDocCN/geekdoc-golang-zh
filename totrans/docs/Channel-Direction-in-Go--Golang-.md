<!--yml
category: 未分类
date: 2024-10-13 06:24:33
-->

# Channel Direction in Go (Golang)

> 来源：[https://golangbyexample.com/channel-direction-go/](https://golangbyexample.com/channel-direction-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Only Send to Channel](#Only_Send_to_Channel "Only Send to Channel")
*   [Only Receive from Channel](#Only_Receive_from_Channel "Only Receive from Channel")*  *# **Overview**

It is possible to create bidirectional as well as uni-directional channels in golang. A channel can be created to which we can only send data, as well as a channel, can be created from which we can only receive data. This is determined by the direction of the arrow of the channel. The direction of the arrow for a channel specifies the direction of flow of data

*   **chan**  :bidirectional channel (Both read and write)

*   **chan <-**  :only writing to channel

*   **<- chan**  :only reading from channel (input channel)    

** A channel to which we can only send data. **

This is the syntax for  such a channel

```
chan<- int
```

**A channel from which we can only send data**

This is the syntax for such a channel

```
<-chan int
```

Now the question is, why would you want to create a channel through to which you can only send data or from which we can only receive data.  This comes in handy while passing the channel to a function where we want to restrict the  function too either  send the data or receiver rate

There are many ways in which a channel can be passed as a function argument.

*   **chan**  :bidirectional channel (Both read and write)
*   **chan <-**  :only writing to channel
*   **<- chan**  :only reading from channel (input channel)    

# **Only Send to Channel**

*   Signature of the such a  channel to which only you can send,  will be like below when passed to a function as an argument.

```
func process(ch chan<- int){ //doSomething }
```

*   When trying to receive data from such a channel will give below error.

```
invalid operation: <-ch (receive from send-only type chan<- int)
```

Try uncommenting below line in the code to see the above error

```
s := <-ch
```

**Code:**

```
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

**Output:** 2

# **Only Receive from Channel**

*   Signature of the such a  channel from which you can only receive data, will be like below  when passed to a function as an argument

```
func process(ch <-chan int){ //doSomething }
```

*   When trying to send data to such a channel will give below error.

```
invalid operation: ch <- 2 (send to receive-only type <-chan int)
```

Try uncommenting below line in the code to see the above error

```
ch <- 2
```

**Code:**

```
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

**Output:** 2

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*