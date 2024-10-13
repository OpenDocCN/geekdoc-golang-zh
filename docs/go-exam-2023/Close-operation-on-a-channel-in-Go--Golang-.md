<!--yml

类别：未分类

日期：2024-10-13 06:24:28

-->

# Go (Golang) 中通道的关闭操作。

> 来源：[`golangbyexample.com/close-operation-on-a-channel-in-go-golang/`](https://golangbyexample.com/close-operation-on-a-channel-in-go-golang/)

关闭是一个内置函数，可以用于关闭通道。关闭通道意味着不能再向通道发送更多数据。当所有数据都已发送且没有更多数据要发送时，通道通常会被关闭。让我们看看一个程序。

```go
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

**输出**

```go
Sum: 6
```

在上面的程序中，我们创建了一个通道。然后在一个 goroutine 中调用了**求和**函数。在主函数中，我们向通道发送了 3 个值，之后关闭了通道，表示不再向通道发送更多值。**求和**函数使用 for 循环遍历通道并计算和。

在一个关闭的通道上发送会导致恐慌。请查看下面的程序。

```go
package main
func main() {
    ch := make(chan int)
    close(ch)
    ch <- 2
}
```

**输出**

```go
panic: send on closed channel
```

关闭一个已经关闭的通道也会导致恐慌。

在从通道接收时，我们还可以使用一个附加变量来确定通道是否已关闭。下面是相应的语法。

```go
val,ok <- ch
```

ok 的值将是

+   如果通道未关闭则为真。

+   如果通道已关闭则为假。

```go
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

**输出**

```go
Val: 2 OK: true
Val: 0 OK: false
```

在上面的程序中创建了一个容量为一的通道。然后我们向通道发送了一个值。第一个接收中的**ok**变量为真，因为通道未关闭。第二个接收中的**ok**变量为假，因为通道已关闭。

**通道上的范围循环**

范围循环可以用来从通道接收数据，直到它被关闭。

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

在上面的程序中，我们创建了一个通道。在主函数中，我们向通道发送了三个值，之后关闭了通道。然后调用了求和函数，并将通道传递给该函数。在求和函数中，我们对通道进行了范围循环。遍历完通道中的所有值后，范围循环将退出，因为通道已关闭。

现在浮现的问题是，如果在主函数中不关闭通道会发生什么。尝试注释掉关闭通道的那一行。现在运行程序。它也会输出死锁，因为范围循环在求和函数中将永远不会结束。

```go
fatal error: all goroutines are asleep - deadlock!
```

让我们看看摘要表，展示不同类型通道关闭操作的结果。

| **命令** | **无缓冲通道**（未关闭且不为 nil） | **缓冲通道**（未关闭且不为 nil） | **已关闭通道** | **空通道** |
| --- | --- | --- | --- | --- |
| 关闭 | 成功 | 成功 | 恐慌 | 恐慌 |
