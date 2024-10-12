<!--yml
category: 未分类
date: 2024-10-13 06:24:41
-->

# All operations/function on a channel in Go (Golang)

> 来源：[https://golangbyexample.com/all-operations-channel-golang/](https://golangbyexample.com/all-operations-channel-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Send Operation](#Send_Operation "Send Operation")
*   [Receive Operation](#Receive_Operation "Receive Operation")
*   [Close operation on a channel](#Close_operation_on_a_channel "Close operation on a channel")
*   [Length of a channel using len() function](#Length_of_a_channel_using_len_function "Length of a channel using len() function")
*   [Capacity of a channel using cap() function](#Capacity_of_a_channel_using_cap_function "Capacity of a channel using cap() function")*  *# **Overview**

Below operations/functions are applicable for channel in golang

*   Send the data to the channel
*   Receive the data from the  channel
*   Close a channel
*   Length of a channel using len() function
*   Capacity of a channel using cap()  function

Let’s look at each operation/functions on a channel one by one

# **Send Operation**

The send operation used to send data to the channel. Below is the format for sending to a channel

```
ch <- data
```

where

*   ch is the channel variable
*   data is what is being sent to the channel

Note that type of data and type of channel should match.
Send operation can block for a buffered or unbuffered channel in the following way

*   **Buffered Channel-** Send on a buffer channel only blocks if the buffer is full

*   **Unbuffered Channel**- Send on a channel is block unless there is another goroutine to receive.

 Let's look at the program for send operation after we have understood the receive operation as well

# **Receive Operation**

The receive operation used to read data from the channel Below is the format for receiving from a channel

```
data := <- ch 
```

where

*   ch is the channel variable

*   data is a variable on which the read data from the channel will be stored.

Let's see an example of where we will send data from one goroutine and receive that data in another goroutine.

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan int)

    fmt.Println("Sending value to channnel")
    go send(ch)

    fmt.Println("Receiving from channnel")
    go receive(ch)

    time.Sleep(time.Second * 1)
}

func send(ch chan int) {
    ch <- 1
}

func receive(ch chan int) {
    val := <-ch
    fmt.Printf("Value Received=%d in receive function\n", val)
}
```

**Output**

```
Sending value to channel
Receiving from channel
Value Received=1 in receive function
```

In the above program, we created a channel that can only transport data of type int. Function **send()** and **receive()** are started as a goroutine. We are sending data to the channel in send() goroutine and receiving data in the receive() goroutine.

Receive operation can block for a buffered or unbuffered channel in the following way

*   **Buffered Channel -** Receiving is only blocked is channel is empty

*   **Unbuffered Channel -** Receiving is blocked until there is another goroutine on the other side to send.

# **Close operation on a channel**

Close is an inbuilt function that can be used to close a channel. Closing of a channel means that no more data can we send to the channel.  Channel is generally closed when all the data has been sent and there's no more data to be send. Let's see a program

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan int)
    go sum(ch, 3)
    ch <- 2
    ch <- 2
    ch <- 2
    close(ch)
    time.Sleep(time.Second * 1)
}

func sum(ch chan int, len int) {
    sum := 0
    for i := 0; i < len; i++ {
        sum += <-ch
    }
    fmt.Printf("Sum: %d\n", sum)
}
```

**Output**

```
Sum: 6
```

In the above program, we created a channel.  Then we called the **sum** function in a goroutine. In the main function, we send 3 values to the channel and after that, we closed the channel indicating that no more values can be sent to the channel. The **sum** function iterates over the channel using the for loop and calculates the sum value.

Sending on a close channel will cause a panic.  See the program below

```
package main

func main() {
    ch := make(chan int)
    close(ch)
    ch <- 2
}
```

**Output**

```
panic: send on closed channel
```

Also closing a already closed channel will cause a panic

While receiving from a  channel we can also use an additional variable  to determine if the channel  has been closed.  Below is the syntax for the same

```
val,ok <- ch
```

The value of ok will be

*   True if the channel is not closed

*   False every channel is closed

```
package main
import (
    "fmt"
)
func main() {
    ch := make(chan int, 1)
    ch <- 2
    val, ok := <-ch
    fmt.Printf("Val: %d OK: %t\n", val, ok)

    close(ch)
    val, ok = <-ch
    fmt.Printf("Val: %d OK: %t\n", val, ok)
}
```

**Output**

```
Val: 2 OK: true
Val: 0 OK: false
```

In the above program created a channel of capacity one.  Then we send one value to the channel.  The **ok** variable in the first receive is true since the channel is not closed. The ok variable in the second  receive is  false because the channel is closed

# **Length of a channel using len() function**

Builtin **len()** function can be used to get the length of a channel. The length of a channel is the number of elements that are already there in the channel. So length actually represents the number of elements queued in the buffer of the channel. Length of a channel is always less than or equal to the capacity of the channel.

Length of unbuffered channel is always zero

```
package main

import "fmt"

func main() {
	ch := make(chan int, 3)
	ch <- 5
	fmt.Printf("Len: %d\n", len(ch))

	ch <- 6
	fmt.Printf("Len: %d\n", len(ch))
	ch <- 7
	fmt.Printf("Len: %d\n", len(ch))
}
```

**Output**

```
Len: 1
Len: 2
Len: 3
```

In the above code, the first created a channel of capacity 3.  After that, we keep sending some value to the channel. As you can notice from your output that after each send operation to the length of channel increases by one as the length denotes the number of item in the buffer of channel.

# **Capacity of a channel using cap() function**

The capacity of a buffered channel is the number of elements that channel can hold. Capacity refers to the size of the buffer of the channel. The capacity of the channel can be specified during the creation of the channel while using the make function. The second argument is the capacity

The capacity of an unbuffered channel is always zero

```
package main

import "fmt"

func main() {
    ch := make(chan int, 3)
    fmt.Printf("Capacity: %d\n", cap(ch))
}
```

**Output**

```
Capacity: 3
```

In the above program we specified the capacity as 3 in the make function

```
make(chan int, 3)
```

Let's see summary table which shows the result of each operation on the different types of channel

| **Command** | **Unbuffered Channel****(Not Closed and not nil)** | **Buffered Channel****(Not Closed and not nil)** | **Closed Channel** | **Nil Channel** |
| Send | Block if there is is no corresponding receiver otherwise success | Block if the channel is full otherwise success | Panic | Block forever |
| Receive | Block if there is no corresponding sender otherwise success | Block if the channel is empty otherwise success | Receives the default value of data type from the channel if channel is empty else  receives the actual value | Block forever |
| Close | Success | Success | Panic | Panic |
| Length | 0 | Number of elements queued in the buffer of the channel | -0 if unbuffered channel-Number of elements queued in the buffer if buffered channel | 0 |
| Capacity | 0 | Size of the buffer of the channel | -0 if unbuffered channel-Size of the buffer if buffered channel | 0 |

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*