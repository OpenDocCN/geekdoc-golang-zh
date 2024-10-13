<!--yml

category: 未分类

日期：2024-10-13 06:23:28

-->

# 在 Go（Golang）中，选择语句与外部 for 循环的结合

> 来源：[https://golangbyexample.com/select-forloop-outside-go/](https://golangbyexample.com/select-forloop-outside-go/)

目录

**   [概述](#Overview "Overview")

+   [有限 for 循环](#Finite_for_loop "Finite for loop")

+   [无限 for 循环](#Infinite_for_loop "Infinite for loop")*  *# **概述**

外部有两种情况下的 for 循环与选择语句

+   外部选择语句的有限 for 循环

+   外部选择语句的无限 for 循环

让我们逐一查看

# **有限 for 循环**

在有限的 for 循环情况下，选择语句的执行次数等于循环中的迭代次数。我们来看一个程序示例

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go goOne(ch1)
    go goTwo(ch2)
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        }
    }
}

func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}

func goTwo(ch chan string) {
    ch <- "From goTwo goroutine"
}
```

**输出**

```go
From goOne goroutine
From goTwo goroutine
```

在上面的程序中，我们在选择语句周围放置了一个长度为二的 for 循环。因此，选择语句执行两次，并打印每个 case 语句接收到的值。

# **无限 for 循环**

我们可以在选择语句外有无限的 for 循环。这将导致选择语句执行无限次。因此，在使用外部无限循环的 for 语句时，我们需要有一种方法来跳出 for 循环。将外部无限循环与选择语句结合的一个用例可能是你在等待多个操作在同一通道上接收一段时间。请看下面的例子

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

在上面的程序中，我们创建了一个名为 **news** 的通道，用于存放字符串类型的数据。然后我们将这个通道传递给 **newsfeed** 函数，该函数将新闻推送到这个通道。在选择语句中，我们正在从 **news** 通道接收新闻。这条选择语句在一个无限的 for 循环中，因此选择语句将被多次执行，直到我们退出 for 循环。我们还在 case 语句中有 **time.After**，其持续时间为 1 秒。所以这个设置将在 1 秒内从 **news** 通道接收所有新闻，然后退出。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
