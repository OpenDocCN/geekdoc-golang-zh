<!--yml
category: 未分类
date: 2024-10-13 06:24:46
-->

# Read/receive all values from a channel in Go (Golang)

> 来源：[https://golangbyexample.com/receive-all-values-channel-golang/](https://golangbyexample.com/receive-all-values-channel-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

For range loop can be used to receive data from the channel until it is closed. Do note that for- range loop will keep receiving from the channel the only way for range look to exit is to close the channel.

Let’s see a program to understand it.

# **Code**

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

 In the above program,  we created a channel.  In the main function the send three values to the channel and after that, we closed the channel. Then we called the sum function and we passed the channel to that function. In the sum function, we did a for range loop over the channel.    After iterating over all the values in the channel the for range loop will exit  since the channel is closed

Now the question which comes to the mind is that what happens if you don't close a channel in the main function.  Try commenting the line in which they are closing the channel. Now run the program.  It will also output  deadlock because  for range loop will never finish in the sum function

```
fatal error: all goroutines are asleep - deadlock!
```

In case we need to receive fixed number of vales from a channel before the channel is closed then we can also use a for loop. Let's see an example for it

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
    sum(ch)
    time.Sleep(time.Second * 1)
}

func sum(ch chan int) {
    sum := 0
    for i := 0; i < 2; i++ {
        val := <-ch
        sum += val
    }
    fmt.Printf("Sum: %d\n", sum)
}
```

**Output**

```
Sum: 4
```

In the above program, we have a  buffered channel of capacity 3\. We are sending 3 values to the channel in the main function. In the sum function, we have a for loop in which we are iterating two times and receiving only two values from the channel and adding it up. For loop is only useful in case we want to receive a fixed number of values from the channel

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*