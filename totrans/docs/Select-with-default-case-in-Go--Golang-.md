<!--yml
category: 未分类
date: 2024-10-13 06:23:33
-->

# Select with default case in Go (Golang)

> 来源：[https://golangbyexample.com/select-default-case-go/](https://golangbyexample.com/select-default-case-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

Similar to switch  select can also have a default case.  This default case will be executed if no send it or receive operation is  ready on any of the case statement. So in a way default statement prevent the select from blocking forever.  So a very important point to note is that the default statement makes the select non-blocking. If the select statement doesn’t contain a default case then it can block forever until one send or receive operation is ready on any case statement. Let’s see a example to fully understand it

# **Code**

```
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    select {
    case msg := <-ch1:
        fmt.Println(msg)
    default:
        fmt.Println("Default statement executed")
    }
}
```

**Output**

```
Default statement executed
```

In the above program have a select statement which is waiting for receive operation on **ch1** and a default statement. Since no goroutine is sending to channel **ch1**, hence the default case is executed and then select exits.

The select statement checks if the value is available in any of the channels in case statements. If it is available  then that case is executed otherwise the default case will be executed immediately. 

Let's see another example where the value is immediately available in one of the channels. We will see that  default case is not executed in this scenario

```
package main

import "fmt"

func main() {
    ch1 := make(chan string, 1)
    ch1 <- "Some value"
    select {
    case msg := <-ch1:
        fmt.Println(msg)
    default:
        fmt.Println("Default statement executed")
    }
}
```

**Output**

```
Some value
```

We can see in the above program that the value was available on the ch1 channel that is why that case is executed and the default case is not executed. Same is also evident from the output

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*