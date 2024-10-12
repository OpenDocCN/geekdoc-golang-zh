<!--yml
category: 未分类
date: 2024-10-13 06:27:08
-->

# What happens during panic in Go (Golang)

> 来源：[https://golangbyexample.com/what-happens-during-panic-go/](https://golangbyexample.com/what-happens-during-panic-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Let’s understand what happens when panic happens in a program. Imagine a function call from **main** function to **f1** function to **f2** function

**main**->**f1**->**f2**

Now let’s say that panic happens in function **f2** then below will be the sequence of events that will be happening

*   Execution of **f2** will stop. Defer functions in **f2** will be executed if present. Control will return to the caller which is a function **f1**.

*   **f1** function will behave in a similar way as if panic happened in that function and after that call will return to the caller which is **main** function. Note that if there are more functions in between then the process will continue up the stack in a similar way

*   **main** function will also behave as if pannic happened in that function and after that, the program will crash

*   Once the program crashes, it will print the panic message along with this stack trace

# **Program**

Let’s see a program for that

```
package main
import "fmt"
func main() {
    f1()
}
func f1() {
    defer fmt.Println("Defer in f1")
    f2()
    fmt.Println("After painc in f1")
}
func f2() {
    defer fmt.Println("Defer in f2")
    panic("Panic Demo")
    fmt.Println("After painc in f2")
}
```

**Output**

```
Defer in f2
Defer in f1
panic: Panic Demo

goroutine 1 [running]:
main.f2()
        main.go:17 +0x95
main.f1()
        main.go:11 +0x96
main.main()
        main.go:6 +0x20
exit status 2
```

In the above program, panic happened in the f2 function like below

```
panic("Panic Demo")
```

the defer function in f2 is called after that and it prints the below message

```
Defer in f2
```

Notice that as soon as the panic happens in the **f2** function, its execution stops therefore below line if **f2** never gets executed

```
fmt.Println("After painc in f2")
```

Control returns to **f1** and it it has a defer function. The defer gets executed and it prints the below message

```
Defer in f1
```

Control returns to main function after that and then the program crashes.The output prints the panic message along with the entire stack trace from main to f1 to f2.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*