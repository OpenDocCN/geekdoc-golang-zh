<!--yml

类别：未分类

日期：2024-10-13 06:24:13

-->

# Go（Golang）中的带超时选择语句

> 来源：[`golangbyexample.com/select-statement-with-timeout-go/`](https://golangbyexample.com/select-statement-with-timeout-go/)

目录

**概述**

+   **代码**

+   在选择语句外使用无限循环的超时

# **概述**

在选择中，可以通过使用时间包的**After()**函数实现超时。下面是**After()**函数的签名。

```go
func After(d Duration) <-chan Time
```

**After**函数等待持续时间 d 完成，然后在通道上返回当前时间 -

[`golang.org/pkg/time/#Time.After`](https://golang.org/pkg/time/#Time.After)

让我们看看一个程序

# **代码**

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch1 := make(chan string)
	go goOne(ch1)

	select {
	case msg := <-ch1:
		fmt.Println(msg)
	case <-time.After(time.Second * 1):
		fmt.Println("Timeout")
	}
}

func goOne(ch chan string) {
	time.Sleep(time.Second * 2)
	ch <- "From goOne goroutine"
}
```

**输出**

```go
Timeout
```

在上述选择语句中，我们正在等待**ch1**上的接收操作完成。在其他案例语句中，我们有**time.After**，持续时间为 1 秒。因此，这个选择语句实际上将等待至少 1 秒以完成**ch1**上的接收操作，之后**time.After**的案例语句将被执行。我们在**goOne**函数中设置了超过 1 秒的超时，因此我们看到**time.After**语句被执行，并且

```go
Timeout
```

被打印为输出。

所以 time.After()是一个通道操作，在一段时间后解除阻塞。

# **在选择语句外使用无限循环的超时**

我们可以在选择语句外使用无限循环。这将导致选择语句执行无限次。因此，在选择语句外使用带无限循环的 for 语句时，我们需要有一种方法来退出循环。选择语句外使用无限循环的一个用例可能是等待多个操作在特定通道上接收一段时间。

请参见以下示例

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	news := make(chan string)
	go newsFeed(news)

	printAllNews(news)
}

func printAllNews(news chan string) {
	for {
		select {
		case n := <-news:
			fmt.Println(n)
		case <-time.After(time.Second * 1):
			fmt.Println("Timeout: News feed finished")
			return
		}
	}
}

func newsFeed(ch chan string) {
	for i := 0; i < 2; i++ {
		time.Sleep(time.Millisecond * 400)
		ch <- fmt.Sprintf("News: %d", i+1)
	}
}
```

**输出**

```go
News: 1
News: 2
Timeout: News feed finished
```

在上述程序中，我们创建了一个名为**news**的通道，它将保存字符串类型的数据。然后，我们将此通道传递给**newsfeed**函数，该函数将新闻推送到此通道。在选择语句中，我们正在从**news**通道接收新闻。这个选择语句位于无限循环中，因此选择语句将多次执行，直到我们退出循环。我们还有一个持续时间为 1 秒的**time.After**作为案例语句之一。因此，该设置将在 1 秒内接收所有来自**news**通道的新闻，然后退出。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
