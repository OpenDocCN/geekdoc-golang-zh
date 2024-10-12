<!--yml
category: 未分类
date: 2024-10-13 06:25:30
-->

# Recover a panic in a different function in Go (Golang)

> 来源：[https://golangbyexample.com/recover-panic-different-function-go/](https://golangbyexample.com/recover-panic-different-function-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

If the **defer** function and **recover** function is not called from the panicking function then it that case **panic** can also be recovered in the called function as well. In fact, it is possible to recover from panic subsequently up in the chain of call stack as wellLet’s see an example of this.

# **Program**

```
package main

import "fmt"

func main() {
    a := []string{"a", "b"}
    checkAndPrintWithRecover(a, 2)
    fmt.Println("Exiting normally")
}

func checkAndPrintWithRecover(a []string, index int) {
    defer handleOutOfBounds()
    checkAndPrint(a, 2)
}

func checkAndPrint(a []string, index int) {
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    fmt.Println(a[index])
}

func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
    }
}
```

**Output**

```
Recovering from panic: Out of bound access for slice
Exiting normally
```

In the above program we have a function **checkAndPrint** which checks and prints slice element at an index passed in the argument. If the index passed is greater than the length of the array then the program panics. 

We have another function **checkAndPrintWithRecover** which contains the call to 

We have another function **checkAndPrintWithRecover** which contains the call to 

*   **defer** function with **recover** which is **handleOutOfBounds**

*   calls **checkAndPrint** function

So we have a defer function named **handleOutIfBounds** at the start of the function **checkAndPrintWithRecover**. This function contains the call to recover function as below

```
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

**checkAndPrint** function raises the panic but doesn’t have the recover function instead the call to recover lies in the **checkAndPrintWithRecover** function. We pass index 2 to the **checkAndPrint** function which is out of bounds index. Hence **checkAndPrint** raises a panic but still, the program is able to recover from panic as seen from the output. This is because panic can be recovered in the called function also and subsequently in the call chain as well

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*