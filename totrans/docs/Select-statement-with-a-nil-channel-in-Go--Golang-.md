<!--yml
category: 未分类
date: 2024-10-13 06:23:43
-->

# Select statement with a nil channel in Go (Golang)

> 来源：[https://golangbyexample.com/select-with-nil-channel-golang/](https://golangbyexample.com/select-with-nil-channel-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

Send or receive operation on nil channel blocks forever. Hence a use case of having a nil channel in the select statement is to disable that case statement after the the send or receive operation is completed on that case statement. The channel then can simply be set to nil. That case statement will be  ignored when the select statement is executed again and receive or send operation will be e waited on another case statement. So it is meant to ignore that case statement and execute the other case statement

# **Code**

```
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

**Output**

```
News: 1
Timeout: News feed finished
```

In the above program, we have created a channel named **news**  which will hold data of string type. Then we pass this channel to the **newsfeed** function which is pushing the news feed to this channel . In the select statement, we are receiving the news feed from the **news** channel. This select statement is inside an infinite for loop  so the select statement will be executed multiple times until we  exit out of for loop . We also have **time.After** with a duration for 1 second as one of the case statements. So this set up will receive all the news from the **news** channel for  1  second and then exit. 

After we receive the first news, we disabled the case statement by setting the news channel to nil.

```
case n := <-news:
   fmt.Println(n)
   news = nil
```

Hence we only received the first news and after that, it times out.  This is the use case of nil channel in the select statement. If we remove the below line

```
news = nil
```

then we will receive all the news in the ouput i.e, output will be

```
News: 1
News: 2
Timeout: News feed finished
```

*   [go](https://golangbyexample.com/tag/go/)*