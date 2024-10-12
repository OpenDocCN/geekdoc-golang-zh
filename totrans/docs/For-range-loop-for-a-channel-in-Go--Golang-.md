<!--yml
category: 未分类
date: 2024-10-13 06:24:50
-->

# For-range loop for a channel in Go (Golang)

> 来源：[https://golangbyexample.com/for-range-loop-channel-go/](https://golangbyexample.com/for-range-loop-channel-go/)

For range loop can be used to receive data from the channel until it is closed. Do note that for- range loop will keep receiving from the channel the only way for range look to exit is to close the channel.

Let’s see a program to understand it

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)