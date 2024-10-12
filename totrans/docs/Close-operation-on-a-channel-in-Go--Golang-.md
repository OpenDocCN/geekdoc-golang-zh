<!--yml
category: 未分类
date: 2024-10-13 06:24:28
-->

# Close operation on a channel in Go (Golang)

> 来源：[https://golangbyexample.com/close-operation-on-a-channel-in-go-golang/](https://golangbyexample.com/close-operation-on-a-channel-in-go-golang/)

Close is an inbuilt function that can be used to close a channel. Closing of a channel means that no more data can we send to the channel.  Channel is generally closed when all the data has been sent and there’s no more data to be send. Let’s see a program

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

While receiving from a  channel we can also use an additional variable to determine if the channel has been closed.  Below is the syntax for the same

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

In the above program created a channel of capacity one.  Then we send one value to the channel.  The **ok** variable in the first receive is true since the channel is not closed. The ok variable in the second  receive is  false because the channel is closed.

**For range loop on a channel**

For range loop can be used to receive data from the channel until it is closed. 

```
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int)
	ch <- 2
	ch <- 2
	ch <- 2
	close(ch)
	sum(ch)
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

Let's see summary table which shows the result of close operation on the different types of channel

| **Command** | **Unbuffered Channel****(Not Closed and not nil)** | **Buffered Channel****(Not Closed and not nil)** | **Closed Channel** | **Nil Channel** |
| Close | Success | Success | Panic | Panic |