<!--yml
category: 未分类
date: 2024-10-13 06:25:35
-->

# Panic with Defer in Go (Golang)

> 来源：[https://golangbyexample.com/panic-with-defer-golang/](https://golangbyexample.com/panic-with-defer-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")
*   [Using defer to recover from a panic](#Using_defer_to_recover_from_a_panic "Using defer to recover from a panic")*  *# **Overview**

**defer** function will be executed even if **panic** happens in a program. In fact, **defer** function is the only function that is called after the **panic.** When the **panic** is raised in a function then the execution of that function is stopped and any deferred function will be executed. In fact deferred function in all the function calls in the stack will also be executed until all the functions have returned. At that time the program will exit and it will print the panic message

So if a defer function is present then it will be executed and the control will be returned back to the caller function which will again execute its defer function if present and the chain goes on until the program exists.

# **Example**

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

In the above program we have a **defer** function first and then we manually create the panic. As you can see from the output that defer function got executed as below line is printed in the output

```
Defer in main
```

# **Using defer to recover from a panic**

Go provides a built-in function **recover** for recovering from a panic. Below is the signature of this function

```
func recover() interface{}
```

**defer** function is the only function that is called after the **panic**. So it makes sense to put the **recover** function in the **defer** function only. If the **recover**  function is not within the defer function then it will not stop **panic**.

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

In the above program we have a function **checkAndPrint** which checks and prints slice element at an index passed in the argument. If the index passed is greater than the length of the array then the program panics.  We have added a defer function named **handleOutIfBounds** as well at the start of the function **checkAndPrint**. This function contains the call to **recover** function as below

```
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

The **recover** function will catch the panic and we can also print the message from the panic. 

```
Recovering from panic: Out of bound access for slice
```

After the **recover** function the program continues and the control returns to the called function which is  **main** here. That is why we get output as

```
Exiting normally
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*