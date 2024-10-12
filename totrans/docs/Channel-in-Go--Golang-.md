<!--yml
category: 未分类
date: 2024-10-13 06:24:19
-->

# Channel in Go (Golang)

> 来源：[https://golangbyexample.com/channel-golang/](https://golangbyexample.com/channel-golang/)

This is the  chapter 24 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Select Statement](https://golangbyexample.com/select-statement-golang/)
**Previous Tutorial** – [Goroutines](https://golangbyexample.com/goroutines-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Declaring Channels](#Declaring_Channels "Declaring Channels")
*   [Operations on Channel](#Operations_on_Channel "Operations on Channel")
    *   [Send Operation](#Send_Operation "Send Operation")
    *   [Receive Operation](#Receive_Operation "Receive Operation")
*   [Channel Direction](#Channel_Direction "Channel Direction")
    *   [Only Send Channel](#Only_Send_Channel "Only Send Channel")
    *   [Only Receive Channel](#Only_Receive_Channel "Only Receive Channel")
*   [Capacity of a channel using cap() function](#Capacity_of_a_channel_using_cap_function "Capacity of a channel using cap() function")
*   [Length of a channel using len() function](#Length_of_a_channel_using_len_function "Length of a channel using len() function")
*   [Close operation on a channel](#Close_operation_on_a_channel "Close operation on a channel")
*   [For range loop on a channel](#For_range_loop_on_a_channel "For range loop on a channel")
*   [Nil channel](#Nil_channel "Nil channel")
*   [Summary table](#Summary_table "Summary table")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Channel is a data type in Go which provides synchrounization and communication between goroutines. They can be thought of as pipes which is used by goroutines to communicate. This communication between goroutines doesn’t require any explicit locks. Locks are internally managed by channel themselves. Channel along with goroutine makes the go programming language concurrent. So we can say that golang has two  concurrency primitives:

*   **Goroutine** – lightweight independent execution to achieve concurrency/parallelism.

*   **Channels** – provides synchronization and communication between goroutines.

# **Declaring Channels**

Each channel variable can hold data only of a particular type. Go uses special keyword **chan** while declaring a channel. Below is the format for declaring a channel

```
var variable_name chan type
```

This only declares a channel which can hold data of type **<type>** and it creates a nil channel as default value of a channel is nil. Let’s see a program to confirm this.

```
package main

import "fmt"

func main() {
    var a chan int
    fmt.Println(a)
}
```

**Output**

```
{nil}
```

To define the channel we can use the inbuilt function **make. **

```
package main

import "fmt"

func main() {
    var a chan int
    a = make(chan int)
    fmt.Println(a)
}
```

**Output**

```
0xc0000240c0
```

On your machine it might give a different address as output.

So what does make do here. A channel is internally represented by a **hchan** struct whose main elements are:

```
type hchan struct {
    qcount   uint           // total data in the queue
    dataqsiz uint           // size of the circular queue
    buf      unsafe.Pointer // points to an array of dataqsiz elements
    elemsize uint16
    closed   uint32         // denotes weather channel is closed or not
    elemtype *_type         // element type
    sendx    uint           // send index
    recvx    uint           // receive index
    recvq    waitq          // list of recv waiters
    sendq    waitq          // list of send waiters
    lock     mutex
}
```

When using **make**, an instance of **hchan** struct is created and all the fields are initialized to their default values.

# **Operations on Channel**

There are two major operations which can be done on a channel 

*   Send

*   Receive

Let’s look at each of it one by one

## **Send Operation**

The send operation is used to send data to the channel. Below is the format for sending to a channel

```
ch <- val
```

where

*   **ch** is the channel variable

*   **val** is what being sent to the channel

Note that data type of **val** and data type of channel should match.

## **Receive Operation**

The receive operation is used to read data from the channel. Below is the format for receiving from  a channel

```
val := <- ch 
```

where

*   **ch** is the channel variable

*   **val** is a variable in which the read data from the channel will be stored.

Let's see an example of where we will send data from one goroutine and receive that data in another goroutine.

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan int)

    fmt.Println("Sending value to channel")
    go send(ch)

    fmt.Println("Receiving from channel")
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

In above program, we created a channel named **ch** whose data type is **int** which means that it can only transport date of type **int**. Function **send()** and **receive()** are started as a goroutine. We are sending data to the channel **ch** in send() goroutine and receiving data from **ch** in the receive() goroutine.

A very important point to note about the receive operation is that a particular value sent to the channel can only be received once in any of the goroutine.  As you can see there are no locks used in the goroutine while sending as well as receiving from the channel. Locks are internally managed by the channels and no explicit lock has to be used in the code. 

By default when we create channel with make, it creates a unbuffered channel which essentially means that channel created cannot store any data. So any send on a channel is blocked until there is  another goroutine to receive it. So in the **send()** function, this line will block

```
ch <- 1
```

until in **receive()** function the value is received

```
val := <-ch
```

Also we have kept a timeout in the main function to allow both send and receive function to complete. If we don't have a timeout in the end of main function, then the program will exit and the two goroutine might not get scheduled.

To illustrate blocking on send lets add a log after we send the value to channel **ch** in the **send()** function and a timeout in the **receive()** function before we receive value from **ch**

```
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int)
	go send(ch)
	go receive(ch)
	time.Sleep(time.Second * 2)
}

func send(ch chan int) {
	ch <- 1
	fmt.Println("Sending value to channel complete")
}

func receive(ch chan int) {
	time.Sleep(time.Second * 1)
	fmt.Println("Timeout finished")
	_ = <-ch
	return
}
```

**Output**

```
Timeout finished
Sending value to channel complete
```

The log

```
Timeout finished
```

will always be before

```
Sending value to channel complete
```

Try out with changing to different values of timeout in the receive function . You will notice the above order always. This illustrates that send on an unbuffered channel is block until a receive happens on that channel in some other goroutine. The receive on a channel is also blocked unless there is another goroutine to send to that channel.

To illustrate blocking on receive lets add a log after we receive the value in the recieve() function and a timeout in the send() function before we send value

```
package main

import (
	"fmt"
	"time"
)

func main() {
  ch := make(chan int)
  go send(ch)

  go receive(ch)
  time.Sleep(time.Second * 2)
}

func send(ch chan int) {
  time.Sleep(time.Second * 1)
  fmt.Println("Timeout finished")
  ch <- 1
}

func receive(ch chan int) {
  val := <-ch
  fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}
```

**Output**

```
Timeout finished
Receiving Value from channel finished. Value received: 1
```

In above program we added a timeout before sending to the channel.

The log

```
Timeout finished
```

will always be before

```
Receiving Value from channel finished. Value received: 1
```

Try out with changing to different values of timeout in the send() function . You will notice the above order always. This illustrates that receive on an unbuffered channel is block until a send happens on that channel in some other goroutine.

We can also receive the value in the main function itself.

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int)
    fmt.Println("Sending value to channel start")
    go send(ch)
    val := <-ch
    fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}

func send(ch chan int) {
    ch <- 1
}
```

**Output**

```
Sending value to channel start
Receiving Value from channel finished. Value received: 1
```

We have seen the example of an unbuffered channel till now. Unbuffered channel does not have any storage hence for an unbuffered channel

*   Send on a channel is block unless there is other goroutine to receive.

*   Receive is block until there is other goroutine on the other side to send.

In Go you can also create a buffered channel. A buffered channel has some capacity to hold data hence for a buffered channel:

*   Send on a buffer channel only blocks if the buffer is full

*   Receive is only block is channel is empty

This is the syntax for creating a buffered channel using the make function

```
a = make(chan <type>, capacity)</type>
```

The second argument specifies the capacity of the channel. Unbuffered channel is of zero capacity .That is why sending is block if there is no receiver and receiving is block if there is no sender for unbuffered channel.

Let's see a program for a buffered channel

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 1)
    ch <- 1
    fmt.Println("Sending value to channnel complete")
    val := <-ch
    fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}
```

In above program we created a buffered channel of length 1 like this

```
ch := make(chan int, 1)
```

We are sending a value and receiving the same in the main goroutine. This is possible as send to a buffered channel is not blocked if the channel is not full. So below line doesn’t block for a buffered channel.

```
ch <- 1
```

The channel is created with a capacity of one. Hence sending to the channel is not blocked and the value is stored in the channel's buffer. So sending and receiving in the same goroutine is only possible for a buffered channel. Let's see two important points we mentioned above

*   Send on a channel is blocked when the channel is full

*   Receive on a channel is blocked when the channel is empty

 Let's see a program for each

**Send on a channel is blocked when the channel is full**

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 1)
    ch <- 1
    ch <- 1
    fmt.Println("Sending value to channnel complete")
    val := <-ch
    fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}
```

**Output**

```
fatal error: all goroutines are asleep - deadlock!
```

In the above program we created a channel of capacity one.  After that, we send one value to the channel and post that we send another value to the channel.

```
ch <- 1
ch <- 1
```

The second sent to the channel is blocked the because buffer is full and hence it results in a deadlock situation because the program cannot proceed and that is why as you can see the output is

```
fatal error: all goroutines are asleep - deadlock!
```

**Receive on a channel is blocked when the channel is empty**

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 1)
    ch <- 1
    fmt.Println("Sending value to channnel complete")
    val := <-ch
    val = <-ch
    fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}
```

**Output**

```
fatal error: all goroutines are asleep - deadlock!
```

In the above program as well we created a channel of capacity one, after that we send one value to the channel and after that, we receive one value from the channel.  Then we tried a second receive from the channel and it resulted in a deadlock situation because the program cannot proceed as the channel is empty and there is nothing to receive. That is why you can see the output is

```
fatal error: all goroutines are asleep - deadlock!
```

This illustrates that the received is blocked if the channel buffer is empty

# **Channel Direction**

So far we have seen bi-directional channels in which we can both send as well as receive data. It is also possible to create uni-directional channels in golang. A channel can be created to which we can only send data, as well as a channel, can be created from which we can only receive data. This is determined by the direction of the arrow of the channel.

*   A channel to which we can only send data. 

This is the syntax for  such a channel

```
chan<- int
```

*   A channel from which we can only send data

This is the syntax for  such a channel

```
<-chan in
```

Now the question is, why would you want to create a channel through to which you can only send data or from which we can only receive data.  This comes in handy while passing the channel to a function where we want to restrict the  function too either  send the data or receiver rate

There are many ways in which a channel can be passed as a function argument. The direction of arrow for a channel specifies the direction of flow of data

*   **chan**  :bidirectional channel (Both read and write)

*   **chan <-**  :only writing to channel

*   **<- chan**  :only reading from channel (input channel)    

## **Only Send Channel**

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

## **Only Receive Channel**

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

# **Capacity of a channel using cap() function**

The capacity of a buffered channel is the number of elements which that channel can hold. Capacity refers to the size of the buffer of the channel. The capacity of the channel can be specified during the creation of the channel while using the make function. The second argument is the capacity

Capacity of unbuffered channel is always zero

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

In the above code, the first created a channel of capacity 3.  After that, we keep sending some value to the channel. As you can notice from your output that after each send operation to the length of channel increases by one as the length denotes the number of items in the buffer of the channel.

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
Sending on a close channel will cause a panic. 

See the program below

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

While receiving from a  channel we can also use an additional variable to determine if the channel has been closed.  Below is the syntax for the  same

```
val,ok <- ch
```

The value of ok will be

*   True if the channel is not closed

*   False if the channel is closed

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

# **For range loop on a channel**

For range loop can be used to receive data from the channel until it is closed. 

```
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int, 3)
	ch <- 2
	ch <- 2
	ch <- 2
	close(ch)
	go sum(ch)
	time.Sleep(time.Second * 1)
}

func sum(ch chan int) {
	sum := 0
	for val := range ch {
		sum += val
	}
	fmt.Printf("Sum: %d\n", sum)
}
```

**Output**

```
Sum: 6
```

 In the above program,  we created a channel.   In the main function the send three values to the channel and after that, we closed the channel. Then we called the sum function and we passed the channel to that function. In the sum function, we did a for range loop over the channel.    After iterating over all the values in the channel the for range loop will exit  since the channel is closed

Now the question which comes to the mind is that what happens if you don't close a channel in the main function.  Try commenting the line in which they are closing the channel. Now run the program.  It will also  output  deadlock because  for range loop will never finish in the sum function

```
fatal error: all goroutines are asleep - deadlock!
```

# **Nil channel**

The zero value of the channel is nil. Hence only declaring a channel creates a nil channel as default zero value of the channel is nil. Let's see a program to demonstrate that

```
package main

import "fmt"

func main() {
    var a chan int
    fmt.Print("Default zero value of channel: ")
    fmt.Println(a)
}
```

**Output**

```
nil
```

Some points to note about nil channel

*   Sending to a  nil channel blocks forever

*   Receiving from nil channel blocks forever

*   Closing a nil channel results in panic

# **Summary table**

So far we have seen 5 operations on a channel.  

*   Send
*   Receive
*   Close
*   Length
*   Capacity

Let's see a summary table which shows the result of each operation on the different types of channel

| **Command** | **Unbuffered Channel****(Not Closed and not nil)** | **Buffered Channel****(Not Closed and not nil)** | **Closed Channel** | **Nil Channel** |
| Send | Block if there is is no corresponding receiver otherwise success | Block if the channel is full otherwise success | Panic | Block forever |
| Receive | Block if there is no corresponding sender otherwise success | Block if the channel is empty otherwise success | Receives the default value of data type from the channel if channel is empty else  receives the actual value | Block forever |
| Close | Success | Success | Panic | Panic |
| Length | 0 | Number of elements queued in the buffer of the channel | -0 if unbuffered channel-Number of elements queued in the buffer if buffered channel | 0 |
| Capacity | 0 | Size of the buffer of the channel | -0 if unbuffered channel-Size of the buffer if buffered channel | 0 |

# **Conclusion**

 This is all about channels in golang .Hope you have liked the article. Please share feedback/improvements/mistakes in comments. 

**Next Tutorial** – [Select Statement](https://golangbyexample.com/select-statement-golang/)
**Previous Tutorial** – [Goroutines](https://golangbyexample.com/goroutines-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*