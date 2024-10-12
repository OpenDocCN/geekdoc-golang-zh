<!--yml
category: 未分类
date: 2024-10-13 06:25:51
-->

# Defer a goroutine in Go (Golang)

> 来源：[https://golangbyexample.com/defer-goroutine-golang/](https://golangbyexample.com/defer-goroutine-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

It is not directly possible to defer a goroutine. But there is a workaround. In the defer function you can call another function in a goroutine like below

```
defer func() {
        go some_function()
}()
```

# **Example**

Let’s see a program for it

```
package main
import (
    "fmt"
    "time"
)
func main() {
    call()
    time.Sleep(time.Second * 2)
}
func call() {
    defer func() {
        go test()
    }()
    fmt.Println("In call function")
}
func test() {
    fmt.Println("In test function")
}
```

**Output**

```
In call function
In test function
```

See how we indirectly defer a goroutine in the **call** function

```
defer func() {
        go test()
}()
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*