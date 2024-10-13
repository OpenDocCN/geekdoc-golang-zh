<!--yml

分类：未分类

日期：2024-10-13 06:23:12

-->

# 在Go(Golang)中接收或获取来自goroutine的多个返回值

> 来源：[https://golangbyexample.com/receive-multiple-return-value-goroutine-golang/](https://golangbyexample.com/receive-multiple-return-value-goroutine-golang/)

通道可以用于从goroutine中获取返回值。通道提供了goroutine之间的同步和通信。你可以在goroutine中将返回值发送到通道，然后在主函数中收集该值。

为了接收多个值，我们可以创建一个自定义结构体类型，其中包含两个返回值的字段，然后创建该类型的通道。

让我们看看一个程序。

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

**输出**

```
Sum Value: 5
Multiply Value: 6
```

在上述程序中，我们创建了一个名为**result**的结构体，它有两个字段。

+   sumValue

+   multiplyValue

我们创建了一个变量**resultChan**，它是一个长度为1的通道，存储**result**结构体类型的值。我们将这个通道传递给**sumAndMultiply**函数。**sumAndMultiply**函数将结果结构体推送到**resultChan**。

```
res := result{sumValue: sumValue, multiplyValue: multiplyValue}
resultChan <- res
```

然后在主函数中，我们正在等待通道以收集结果。

```
res := <-resultChan
```

这行代码将等待，直到一个值被推送到**resultChan**通道。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
