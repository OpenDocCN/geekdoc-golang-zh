<!--yml
category: 未分类
date: 2024-10-13 06:24:08
-->

# Empty select or select with no case in Go (Golang)

> 来源：[https://golangbyexample.com/empty-select-golang/](https://golangbyexample.com/empty-select-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

Select block without any case statement is empty select. The empty select will block forever as there is no case statement  to execute. We know that select statement gets blocked until any of the case statement can be executed. It is also one of the way for a goroutine to wait indefinitely. But if this empty select is put in the main goroutine then it will cause a deadlock.

Let’s see a program

# **Code**

```
package main

func main() {
    select {}
}
```

**Output**

```
fatal error: all goroutines are asleep - deadlock!
```

In the above program we have empty select and hence it results in a deadlock that is why you see the output as below

```
fatal error: all goroutines are asleep - deadlock!
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*