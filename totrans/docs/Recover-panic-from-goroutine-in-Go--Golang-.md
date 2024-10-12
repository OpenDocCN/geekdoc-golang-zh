<!--yml
category: 未分类
date: 2024-10-13 06:26:01
-->

# Recover panic from goroutine in Go (Golang)

> 来源：[https://golangbyexample.com/recover-panic-goroutine-go/](https://golangbyexample.com/recover-panic-goroutine-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Recover function in the same goroutine as panic](#Recover_function_in_the_same_goroutine_as_panic "Recover function in the same goroutine as panic")
*   [Recover function in a different goroutine as panic](#Recover_function_in_a_different_goroutine_as_panic "Recover function in a different goroutine as panic")*  *# **Overview**

There are two cases for recovering from a panic in a goroutine

*   **recover** function in the same goroutine as **panic**

*   **recover** function in a different goroutine as **panic**

While in the first case it will recover from panic. But an important point to note about recover function is that it can only recover the panic happening in the same goroutine.  If the panic is happening in a different goroutine and recover is in a different goroutine then it.

Let’s see an example for both

# ****Recover function in the same goroutine as panic****

```
package main

import "fmt"

func main() {
    a := []string{"a", "b"}
    go checkAndPrintWithRecover(a, 2)
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
Exiting normally
Recovering from panic: Out of bound access for slice
```

In the above program **recover** and **panic** lie in the same goroutine hence it is able to recover from panic as seen from the output.

# **Recover function in a different goroutine as panic**

As mentioned in this case it is not possible to recover from panic.

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
    go checkAndPrint(a, 2)
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
Exiting normally
panic: Out of bound access for slice

goroutine 18 [running]:
main.checkAndPrint(0xc0000a6020, 0x2, 0x2, 0x2)
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/goroutine/main.go:19 +0xe2
created by main.checkAndPrintWithRecover
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/goroutine/main.go:14 +0x82
exit status 2
```

In the above program we have **checkAndPrint** in the goroutine and it raises **panic** in that goroutine.  The **recover** function is in the calling goroutine. As you can see from the output that it does not stop **panic** and hence you see the a

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*