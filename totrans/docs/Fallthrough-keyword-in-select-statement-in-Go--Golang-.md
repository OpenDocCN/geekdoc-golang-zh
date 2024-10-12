<!--yml
category: 未分类
date: 2024-10-13 06:23:53
-->

# Fallthrough keyword in select statement in Go (Golang)

> 来源：[https://golangbyexample.com/fallthrough-keyword-select-golang/](https://golangbyexample.com/fallthrough-keyword-select-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

Select doesn’t allow **fallthrough** keyword to select multiple cases. **Fallthrough** keyword can only be used in  switch statement to select multiple cases Only one case out of ready cases will be chosen at random. It cannot execute multiple cases but there is a workaround to it. We can have a for loop outside the set statement. This for loop will call the select statement equal to the number of iterations in the loop.

Let see an example.

# **Code**

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

This way we can execute multiple  case statement but do note that it is not deterministic.  If there are multiple operations available on the same channel for a particular case statement then that case can be executed every time in all iterations of the for loop

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*