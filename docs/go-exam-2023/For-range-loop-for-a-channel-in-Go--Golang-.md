<!--yml

分类：未分类

日期：2024-10-13 06:24:50

-->

# Go（Golang）中的通道范围循环

> 来源：[`golangbyexample.com/for-range-loop-channel-go/`](https://golangbyexample.com/for-range-loop-channel-go/)

范围循环可以用于从通道接收数据，直到它被关闭。请注意，范围循环将持续从通道接收数据，退出的唯一方式是关闭通道。

让我们看看一个程序来理解它。

```go
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

**输出**

```go
Sum: 6
```

在上述程序中，我们创建了一个通道。在主函数中发送三个值到通道，然后关闭了通道。接着我们调用了求和函数，并将通道传递给该函数。在求和函数中，我们对通道进行了一个范围循环。在迭代通道中的所有值后，由于通道已关闭，范围循环将退出。

现在脑海中出现的问题是，如果你在主函数中不关闭一个通道会发生什么。尝试注释掉关闭通道的那一行。现在运行这个程序。它也会输出死锁，因为对于范围循环在求和函数中将永远不会结束。

```go
fatal error: all goroutines are asleep - deadlock!
```


