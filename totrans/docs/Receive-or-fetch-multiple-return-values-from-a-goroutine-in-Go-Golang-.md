<!--yml
category: 未分类
date: 2024-10-13 06:23:12
-->

# Receive or fetch multiple return values from a goroutine in Go(Golang)

> 来源：[https://golangbyexample.com/receive-multiple-return-value-goroutine-golang/](https://golangbyexample.com/receive-multiple-return-value-goroutine-golang/)

Channels can be used to fetch return value from a goroutine. Channels provide synchronization and communication between goroutines. You can send the return value to a channel in the goroutine and then collect that value in the main function.

To receive multiple values we can create a  custom struct type with fields for both return values, then create a channel of that type.

Let’s see a program

```
package main

import (
	"fmt"
	"time"
)

type result struct {
	sumValue      int
	multiplyValue int
}

func main() {
	resultChan := make(chan result, 1)
	sumAndMultiply(2, 3, resultChan)

	res := <-resultChan
	fmt.Printf("Sum Value: %d\n", res.sumValue)
	fmt.Printf("Multiply Value: %d\n", res.multiplyValue)
	close(resultChan)

}

func sumAndMultiply(a, b int, resultChan chan result) {
	sumValue := a + b
	multiplyValue := a * b
	res := result{sumValue: sumValue, multiplyValue: multiplyValue}
	time.Sleep(time.Second * 2)
	resultChan <- res
	return
}
```

**Output**

```
Sum Value: 5
Multiply Value: 6
```

In the above program we created a struct named **result** which has two fields

*   sumValue

*   multiplyValue

We then created a variable **resultChan** which is a channel of length 1 holding value of **result** struct type. We passed this channel to the **sumAndMultiply** function. The **sumAndMultiply** function pushes the resultant struct to the **resultChan**

```
res := result{sumValue: sumValue, multiplyValue: multiplyValue}
resultChan <- res
```

Then in the main function we are waiting on the channel to collect the result

```
res := <-resultChan
```

This line will wait until a value is pushed to the **resultChan** channel.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)