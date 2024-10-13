<!--yml

分类：未分类

日期：2024-10-13 06:23:43

-->

# 在 Go (Golang) 中使用 nil 通道的 select 语句

> 来源：[`golangbyexample.com/select-with-nil-channel-golang/`](https://golangbyexample.com/select-with-nil-channel-golang/)

目录

+   概述

+   代码

# **概述**

在 nil 通道上进行发送或接收操作会永久阻塞。因此，在 select 语句中使用 nil 通道的一个用例是，在该 case 语句上的发送或接收操作完成后禁用该 case 语句。然后通道可以简单地设置为 nil。当 select 语句再次执行时，该 case 语句将被忽略，接收或发送操作将会在另一个 case 语句上等待。因此，它的目的是忽略该 case 语句并执行其他 case 语句。

# **代码**

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
            news = nil
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
Timeout: News feed finished
```

在上面的程序中，我们创建了一个名为 **news** 的通道，该通道将保存字符串类型的数据。然后我们将这个通道传递给 **newsfeed** 函数，该函数将新闻推送到这个通道。在 select 语句中，我们正在从 **news** 通道接收新闻。这条 select 语句在一个无限的 for 循环内部，因此 select 语句将被多次执行，直到我们退出 for 循环。我们还有 **time.After**，持续时间为 1 秒，作为其中一个 case 语句。因此，这个设置将在 **news** 通道中接收所有新闻，持续 1 秒后退出。

在接收第一条新闻后，我们通过将新闻通道设置为 nil 来禁用该 case 语句。

```go
case n := <-news:
   fmt.Println(n)
   news = nil
```

因此，我们只接收到第一条消息，之后就超时了。这是 select 语句中 nil 通道的用例。如果我们移除下面的行

```go
news = nil
```

然后我们将在输出中接收所有新闻，即输出将是

```go
News: 1
News: 2
Timeout: News feed finished
```

+   [go](https://golangbyexample.com/tag/go/)*
