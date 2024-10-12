<!--yml
category: 未分类
date: 2024-10-13 06:26:27
-->

# Panic stack trace in Go (Golang)

> 来源：[https://golangbyexample.com/panic-stack-trace-go/](https://golangbyexample.com/panic-stack-trace-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

**debug** package of golang provides a **StackTrace** function that can be used print the stack trace of the panic in the recover function

# **Example**

Let’s see a program for that

```
package main
import (
    "fmt"
    "runtime/debug"
)
func main() {
    a := []string{"a", "b"}
    checkAndPrint(a, 2)
    fmt.Println("Exiting normally")
}
func checkAndPrint(a []string, index int) {
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    fmt.Println(a[index])
}
func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
        fmt.Println("Stack Trace:")
        debug.PrintStack()
    }
}
```

**Output**

```
Recovering from panic: Out of bound access for slice
Stack Trace:
goroutine 1 [running]:
runtime/debug.Stack(0xd, 0x0, 0x0)
        stack.go:24 +0x9d
runtime/debug.PrintStack()
        stack.go:16 +0x22
main.handleOutOfBounds()
        main.go:27 +0x10f
panic(0x10ab8c0, 0x10e8f60)
        /Users/slohia/Documents/goversion/go1.14.1/src/runtime/panic.go:967 +0x166
main.checkAndPrint(0xc000104f58, 0x2, 0x2, 0x2)
        main.go:18 +0x111
main.main()
        main.go:11 +0x81
Exiting normally
```

In the above program we have a function **checkAndPrint** which checks and prints slice element at an index passed in the argument. If the index passed is greater than the length of the array then the program panics.  We have added a defer function named **handleOutIfBounds** as well at the start of the function **checkAndPrint**. This function contains the call to recover function as below

```
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

The **recover** function will catch the panic and we can also print the message from the panic. 

```
Recovering from panic: Out of bound access for slice
```

We print the stack trace of the panic in the recover function using the **StackTrace** function. It prints the correct  stack trace as seen from the output

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*