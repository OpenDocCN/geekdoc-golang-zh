<!--yml

category: 未分类

日期：2024-10-13 06:24:46

-->

# 在 Go (Golang) 中从通道读取/接收所有值

> 来源：[https://golangbyexample.com/receive-all-values-channel-golang/](https://golangbyexample.com/receive-all-values-channel-golang/)

目录

**   [概述](#Overview "Overview")**

+   [代码](#Code "Code")*  *# **概述**

对于范围循环，可以用来接收来自通道的数据，直到它被关闭。请注意，for-range 循环会持续接收通道中的数据，唯一退出范围循环的方式是关闭通道。

让我们来看一个程序以理解这一点。

# **代码**

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

**输出**

```
Sum: 6
```

在上面的程序中，我们创建了一个通道。在主函数中，发送了三个值到通道，然后关闭了通道。接着我们调用了 sum 函数，并将通道传递给该函数。在 sum 函数中，我们对通道进行了 for-range 循环。在遍历完通道中的所有值后，由于通道已关闭，for-range 循环将退出。

现在脑海中浮现的问题是，如果在主函数中不关闭通道会发生什么。尝试注释掉关闭通道的那一行。然后运行程序。它也会输出死锁，因为 for-range 循环在 sum 函数中永远不会结束。

```
fatal error: all goroutines are asleep - deadlock!
```

如果我们需要在通道关闭前接收固定数量的值，可以使用 for 循环。让我们看一个例子。

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

**输出**

```
Sum: 4
```

在上面的程序中，我们有一个容量为 3 的缓冲通道。在主函数中，我们向通道发送了 3 个值。在 sum 函数中，我们有一个 for 循环，迭代两次，只从通道中接收两个值并进行相加。只有在我们想从通道中接收固定数量的值时，for 循环才有用。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
