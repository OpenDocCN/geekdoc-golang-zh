<!--yml
category: 未分类
date: 2024-10-13 06:08:53
-->

# channel as function argument in Go (Golang)

> 来源：[https://golangbyexample.com/channel-function-argument-go/](https://golangbyexample.com/channel-function-argument-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Bidirectional channel](#Bidirectional_channel "Bidirectional channel")
*   [Only Send Channel](#Only_Send_Channel "Only Send Channel")
*   [Only Receive Channel](#Only_Receive_Channel "Only Receive Channel")
*   [Channel Pointer](#Channel_Pointer "Channel Pointer ")*  *# Overview

There are many ways in which a channel can be passed as a function argument. The direction of the arrow for a channel specifies the direction of flow of data

*   **chan** : bidirectional channel (Both read and write)

*   **chan <-** : only writing to channel

*   **<- chan** : only reading from the channel (input channel)

*   ***chan** : channel pointer. Both read and write

# **Bidirectional channel**

Signature of such a bidirectional channel will be like below when passed to a function as an argument.

```
func process(ch chan int){ //doSomething }
```

**Code:**

```
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

**Output:** 2

# **Only Send Channel**

*   Signature of such a  channel to which only you can send will be like below when passed to a function as an argument.

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

# **Only Receive Channel**

*   Signature of the such a  channel from which you can only receive data will be like below  when passed to a function as an argument

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

# **Channel Pointer**

This way of passing a channel would only make sense if you would want to modify the channel. This is very uncommon and not a preferable way to use. Signature of the such a channel which is passed as a pointer

```
func process(ch *chan int){ //doSomething}
```

**Code:**

```
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

**Output:** 2*