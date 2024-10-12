<!--yml
category: 未分类
date: 2024-10-13 06:23:07
-->

# Receive or Fetch Return Value from a goroutine in Go(Golang)

> 来源：[https://golangbyexample.com/return-value-goroutine-go/](https://golangbyexample.com/return-value-goroutine-go/)

Channels can be used to fetch return value from a goroutine. Channels provide synchronization and communication between goroutines. You can send the return value in a channel in the goroutine and then collect that value in the main function.

Let’s see a program

```
package main

import "fmt"

func main() {
	result := make(chan int, 1)
	go sum(2, 3, result)

	value := <-result
	fmt.Printf("Value: %d\n", value)
	close(result)

}

func sum(a, b int, result chan int) {
	sumValue := a + b
	result <- sumValue
	return
}
```

**Output**

```
Value: 5
```

In the above program we created a variable **result** which is a channel of length 1 holding value of **int** type. We passed this channel to the **sum** function. The **sum** function pushes the **sumValue** to the **result** channel like this

```
result <- sumValue
```

The in the main function we are waiting on the **result** channel to collect the result

```
value := <-result
```

This line will wait until a value is pushed to the **result** channel. As seen above the **sumValue** will be pushed to **result** channel by the function **sum**.

To illustrate that this line will actually wait let's put a timeout in the sum function it actually pushes **sumValue** to the **result** channel

```
package main

import (
	"fmt"
	"time"
)

func main() {
	result := make(chan int, 1)
	go sum(2, 3, result)

	value := <-result
	fmt.Printf("Value: %d\n", value)
	close(result)

}

func sum(a, b int, result chan int) {
	sumValue := a + b
	time.Sleep(time.Second * 2)
	result <- sumValue
	return
}
```

**Output**

```
Value: 5
```

The program gives the same output which proves that the **main** function waits for **sumValue** to be pushed to the **result** channel. Once it receives the value from the **result** channel, it prints it.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)