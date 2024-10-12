<!--yml
category: 未分类
date: 2024-10-13 06:23:28
-->

# Select statement with a for loop outside in Go (Golang)

> 来源：[https://golangbyexample.com/select-forloop-outside-go/](https://golangbyexample.com/select-forloop-outside-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Finite for loop](#Finite_for_loop "Finite for loop")
*   [Infinite for loop](#Infinite_for_loop "Infinite for loop")*  *# **Overview**

There are two cases of for loop being outside a select statement

*   Finite for loop outside select statement
*   Infinite for loop outside select statement

Let’s look at each one by one

# **Finite for loop**

In case of finite for loop, the select statement will execute equal to the number of iterations in the loop. Let’s see a program for that

```
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

**Output**

```
From goOne goroutine
From goTwo goroutine
```

In the above program  we put a for loop of length two across the select statement. Hence  the select statement is executed twice and   prints the received value from each of the case statement.

# **Infinite for loop**

We can have infinite for loop outside a select statement. This will cause the select statement to execute indefinite number of times.So when using for statement with infinite loop outside the select statement, we need to have a way yo break out of the for loop. One of the use case of having infinite for loop outside select statement could be that you are waiting for multiple operations to receive on the same channel for a certain time. See below example

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
News: 2
Timeout: News feed finished
```

In the above program, we have created a channel named **news**  which will hold data of string type. Then we pass this channel to the **newsfeed** function which is pushing the news feed to this channel . In the select statement, we are receiving the news feed from the **news** channel. This select statement is inside an infinite for loop  so the select statement will be executed multiple times until we  exit out of for loop . We also have **time.After** with a duration for 1 second as one of the case statements. So this set up will receive all the news from the **news** channel for  1  second and then exit. 

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*