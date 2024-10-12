<!--yml
category: 未分类
date: 2024-10-13 06:26:06
-->

# Panic and Recover in Go (Golang)

> 来源：[https://golangbyexample.com/panic-and-recover-golang/](https://golangbyexample.com/panic-and-recover-golang/)

This is the  chapter 28 and also the last chapter of the golang comprehensive tutorial series. This is Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Previous Tutorial** – [Error -Part 2](https://golangbyexample.com/error-in-golang-advanced/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Runtime Error Panic](#Runtime_Error_Panic "Runtime Error Panic")
*   [Calling the panic function explicitly](#Calling_the_panic_function_explicitly "Calling the panic function explicitly")
*   [Panic with defer](#Panic_with_defer "Panic with defer")
*   [Recover in golang](#Recover_in_golang "Recover in golang")
*   [Panic/Recover and Goroutine](#PanicRecover_and_Goroutine "Panic/Recover and Goroutine")
*   [Printing stack trace](#Printing_stack_trace "Printing stack trace")
*   [Return value of the function when panic is recovered](#Return_value_of_the_function_when_panic_is_recovered "Return value of the function when panic is recovered")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Panic in golang is similar to the exception. Panic is meant to exit from a program in abnormal conditions. Panic can occur in a program in two ways

*   Runtime error in the program
*   By calling the panic function explicitly. This can be called by the programmer when the program cannot continue and it has to exit

Go provides a special function to create a panic. Below is the syntax of the function

```
func panic(v interface{})
```

This function can be called explicitly by the programmer to create a panic. It takes an empty interface as an argument.  When a panic happens in a  program it outputs two things

*   The error message that is passed to the panic function as an argument
*   Stack trace of where the panic happened

# **Runtime Error Panic**

Runtime error in the program can happen in below cases

*   Out of bounds array access

*   Calling a function on a nil pointer

*   Sending on a closed channel

*   Incorrect type assertion

Let’s see an example of runtime error caused by out of bounds array access.

```
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	print(a, 2)
}

func print(a []string, index int) {
	fmt.Println(a[index])
}
```

**Output**

```
panic: runtime error: index out of range [2] with length 2

goroutine 1 [running]:
main.checkAndPrint(...)
        main.go:12
main.main()
        /main.go:8 +0x1b
exit status 2
```

In the above program we have a slice of length 2 and we are trying to access slice at index 3 in the **print** function. Out of bound access is not allowed and it will create panic as seen from the output. Notice that in the output there are two things

*   The error message
*   Stack trace of where the panic happened

There are many more cases in which runtime error can happen in a program. We are not going to mention all of them but you get the idea

# **Calling the panic function explicitly**

Some of the cases where panic function can be called explicitly by the programmer are:

*   The function expected a valid argument but instead, a nil argument was supplied. In such a case, the program cannot continue and it will raise a panic for a nil argument passed

*   Any other scenario in which the program cannot continue.

Let’s see an example.

```
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
}

func checkAndPrint(a []string, index int) {
	if index > (len(a) - 1) {
		panic("Out of bound access for slice")
	}
	fmt.Println(a[index])
}
```

**Output**

```
panic: Out of bound access for slice

goroutine 1 [running]:
main.checkAndPrint(0xc000104f58, 0x2, 0x2, 0x2)
      main.go:13 +0xe2
main.main()
        main.go:8 +0x7d
exit status 2
```

In the above program, we again have a function **checkAndPrint** which accepts a slice as an argument and an index. Then it checks whether the index passed is greater than the length of slice minus 1\. If it is, then it is out of bounds access for the slice so it panics. If not then it prints the value at that index. Again notice that in the output there are two things

*   The error message

*   Stack trace of where the panic happened

# **Panic with defer**

When the panic is raised in a function then the execution of that function is stopped and any deferred function will be executed. In fact deferred function of all the function calls in the stack will also be executed until all the functions have  returned .At that time the program will exit and it will print the panic message

So if a  defer function is present it then it will be executed and the control will be  returned back to the caller function which will again execute its defer function if present and the chain goes on until the program exists.

Let’s see an example

```
package main
import "fmt"
func main() {
    defer fmt.Println("Defer in main")
    panic("Panic with Defer")
    fmt.Println("After painc in f2")
}
```

**Output**

```
Defer in main
panic: Panic Create

goroutine 1 [running]:
main.main()
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/deferWithPanic/main.go:7 +0x95
exit status 2
```

In the above program we have a defer function first and then we manually create the panic. As you can see from the output that defer function got executed as below line is printed in the output

```
Defer in main
```

Let’s understand what happens when panic happens in a program. Imagine a function call from **main** function to **f1** function to **f2** function

**main**->**f1**->**f2**

Now let’s say that panic happens in function **f2** then below will be the sequence of events that will be happening

*   Execution of **f2** will stop. Defer functions in **f2** will be executed if present. Control will return to the caller which is a function **f1**.

*   **f1** function will behave in a similar way as if panic happened in that function and after that call will return to the caller which is **main** function. Note that if there are more functions in between then the process will continue up the stack in a similar way

*   **main** function will also behave as if panic happened in that function and after that, the program will crash

*   Once the program crashes, it will print the panic message along with this stack trace

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

Control returns to **f1** and it it has a defer function. The defer gets executed and it prints the below message.

```
Defer in f1
```

Control returns to main function after that and then the program crashes.The output prints the panic message along with the entire stack trace from main to f1 to f2.

# **Recover in golang**

Go provides a built-in function **recover** for recovering from a panic. Below is the signature of this function

```
func recover() interface{}
```

We already learn above that **defer** function is the only function that is called after the **panic**. So it makes sense to put the **recover** function in the **defer** function only. If the **recover** function is not within the defer function then it will not stop **panic**.

Let’s see an example of recover

```
package main

import "fmt"

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
	}
}
```

**Output**

```
Recovering from panic: Out of bound access for slice
Exiting normally
```

In the above program we have a function **checkAndPrint** which checks and prints slice element at an index passed in the argument. If the index passed is greater than the length of the array then the program panics.  We have added a defer function named **handleOutIfBounds** as well at the start of the function **checkAndPrint**.   This function  contains  the  call to recover function as below

```
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

The **recover** function will catch the panic and we can also print the message from the panic. 

```
Recovering from panic: Out of bound access for slice
```

After the recover function the program continues and the control returns to the called function which is  **main** here. That is why we get output as

```
Exiting normally
```

The recover function returns the value which was passed to the panic function. Therefore it is a good practice to check the return value of the recover function. If the return value is nil then panic did not happen and recover function was not called with the panic. That is why we have below code in the  defer function **handleOutofBounds**

```
if r := recover(); r != nil 
```

Here if **r** is nil then panic did not happened. So if there is no panic then call to recover will return nil

Note that if the defer function and recover function is not called from the panicking function then it that case also panic can be recovered in the called function as well. Infact it is possible to recover from panic subsequently up in the chain of call stack.

Let’s see an example of this

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

The above program is quite the same as the previous program other than we have an additional function **checkAndPrintWithRecover** which contains the call to 

*   defer function with recover which is **handleOutOfBounds**
*   calls **checkAndPrint** function

**So basically **checkAndPrint** function raises the panic but doesn’t have the recover function instead call to recover lies in the **checkAndPrintWithRecover** function. But still the program is able to recover from panic  as panic can also be recovered in the called function also and subsequently in the chain as well**

 **We mentioned above that if the recover function is not within defer function then it will not stop the panic.

Let’s see an example program for that

```
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
	fmt.Println("Exiting normally")
}

func checkAndPrint(a []string, index int) {
	handleOutOfBounds()
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
panic: Out of bound access for slice

goroutine 1 [running]:
main.checkAndPrint(0xc000104f58, 0x2, 0x2, 0x2)
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/recoverNegativeExample/main.go:15 +0xea
main.main()
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/recoverNegativeExample/main.go:8 +0x81
exit status 2
```

In the above program the recover function  is not within defer function.  as you can see from the output that it does not stop panic and hence  you see the above output

# **Panic/Recover and Goroutine**

An important point to note about be recover function is that it can only recover the panic happening in the same goroutine.  If the panic is happening in different  goroutine and recover is in a different goroutine then it won’t stop  panic. Lets see a program for that

```
package main
import "fmt"
func main() {
    a := []string{"a", "b"}
    checkAndPrintWithRecover(a, 2)
    time.Sleep(time.Second)
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

In the above program we have **checkAndPrint** in the goroutine and it raises **panic** in that goroutine.  The **recover** function is in the calling goroutine. As you can see from the output that it does not stop **panic** and hence  you see the above output

# **Printing stack trace**

Debug package of golang also provide StackTrace function that can be used print the stack trace of the panic in the recover function

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

In the above program we print the stack trace of the panic in the recover function using the **StackTrace** function. It prints the correct  stack trace as seen from the output

# **Return value of the function when panic is recovered**

When the panic is recovered then the return value of a panicking function will be the default value of the return types of  the panicking function

Let’s see a program for it

```
package main
import (
    "fmt"
)
func main() {
    a := []int{5, 6}
    val, err := checkAndGet(a, 2)
    fmt.Printf("Val: %d\n", val)
    fmt.Println("Error: ", err)
}
func checkAndGet(a []int, index int) (int, error) {
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    return a[index], nil
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
Val: 0
Error: 
```

In the above program we have **checkAndGet** function which gets the value at a particular index in int slice. If the index passed to this function is greater than (length of slice-1), then it raises a panic. There is also a **handleOutOfBounds** function which is used to recover from the panic.  So we pass index 2 to the **checkAndGet** function and it raises the panic which is recovered in the **handleOutOfBounds** function. That is why we first get this output

```
Recovering from panic: Out of bound access for slice
```

Notice in main function that we recollect the return value from the **checkAndGet** like this

```
val, err := checkAndGet(a, 2)
```

**checkAndGet** has two return values

*   int
*   error

Since **checkAndGet** creates panic which is recovered in the handleOutOfBounds function therefore the return value of the **checkAndGet** will be the default value of its types.

Therefore

```
 fmt.Printf("Val: %d\n", val)
```

outputs

```
Val: 0
```

because zero is the default value of **int** type.

And

```
fmt.Println("Error: ", err)
```

outputs

```
Error: 
```

because nil is the default value of **error** type.

If you don’t want to return default zero value of types then named return value can be used. Let’s see a program for that.

```
package main
import (
    "fmt"
)
func main() {
    a := []int{5, 6}
    val, err := checkAndGet(a, 2)
    fmt.Printf("Val: %d\n", val)
    fmt.Println("Error: ", err)
}
func checkAndGet(a []int, index int) (value int, err error) {
    value = 10
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    value = a[index]
    return value, nil
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
Val: 10
Error: 
```

This program is same as previous program, the only difference being that we are using named return value in the **checkAndGet** function.

```
func checkAndGet(a []int, index int) (value int, err error)
```

We set the named return value to 10 in **checkAndGet** function

```
value = 10
```

That is why we get below output in this program as panic is created and it is recovered

```
Recovering from panic: Out of bound access for slice
Val: 10
Error: 
```

Also note that If panic would not have created in the program then it would have output the correct value at index.

# **Conclusion**

That is all about panic and recover in golang. Hope you have liked the article. please share feedback/improvements/mistakes in comments

**Previous Tutorial** – [Error -Part 2](https://golangbyexample.com/error-in-golang-advanced/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)***