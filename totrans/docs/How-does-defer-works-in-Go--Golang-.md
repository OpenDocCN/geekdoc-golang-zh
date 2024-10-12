<!--yml
category: 未分类
date: 2024-10-13 06:27:03
-->

# How does defer works in Go (Golang)

> 来源：[https://golangbyexample.com/how-defer-works-golang/](https://golangbyexample.com/how-defer-works-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Single Defer](#Single_Defer "Single Defer")
*   [Multiple Defer function within a particular function](#Multiple_Defer_function_within_a_particular_function "Multiple Defer function within a particular function")
*   [Multiple Defer function in different functions](#Multiple_Defer_function_in_different_functions "Multiple Defer function in different functions")*  *# **Overview**

When the compiler encounters a defer statement in a function it pushes it onto a list. This list internally implements a stack-like data structure.  All the encountered defer statement in the same function are pushed onto this list. When the surrounding function returns then all the functions in the stack starting from top to bottom are executed before execution can begin in the calling function. Now the same thing will happen in the calling function as well.

Let’s see three cases of defer to better understand how defer works in go

*   Single defer
*   Multiple defer in the same function
*   Different defer in different function

Let’s see an example of each of them

# **Single Defer**

```
package main
import "fmt"
func main() {
    defer test()
    fmt.Println("Executed in main")
}
func test() {
    fmt.Println("In Defer")
}
```

**Output**

```
Executed in main
In Defer
```

In the above program there is a **defer** statement calling the custom function named **test**. As seen from the output, the **test** function is called after everything in the main is executed and before main returns. That is why

```
Executed in main
```

is printed before

```
In Defer
```

# **Multiple Defer function within a particular function**

In case we have multiple defer functions within a particular function, then all the  defer functions will be executed in last in first out order which is similar to what we mentioned above

Let’s see a program for that

```
package main
import "fmt"
func main() {
    i := 0
    i = 1
    defer fmt.Println(i)
    i = 2
    defer fmt.Println(i)
    i = 3
    defer fmt.Println(i)
}
```

**Output**

```
3
2
1
```

In the above program we have three **defer** function each printing the value of **i**. The variable **i** is incremented before each defer. The code outputs 3 first meaning that third defer function is executed first.  Then it outputs 2 meaning that second defer is executed after that and then it outputs 1 meaning that first defer is executed last. This shows that when there are multiple defer functions within a particular function then the follow the “Last in first out” rule. And that is why program outputs 

```
3
2
1
```

# **Multiple Defer function in different functions**

Let’s understand what happens when we have multiple defer functions in different functions.. Imagine a function call from **main** function to **f1** function to **f2** function

**main**->**f1**->**f2**

Below is the sequence that will be happening after f2 returns

*   Defer functions in **f2** will be executed if present. Control will return to the caller which is a function **f1**.

*   Defer functions in **f1** will be executed if present. Control will return to the caller which is a function **main**. Note that if there are more functions in between then the process will continue up the stack in a similar way

*   After main returns the defer  function if present in main will be executed

Let’s see a program for that

```
package main

import "fmt"

func main() {
	defer fmt.Println("Defer in main")
	fmt.Println("Stat main")
	f1()
	fmt.Println("Finish main")
}

func f1() {
	defer fmt.Println("Defer in f1")
	fmt.Println("Start f1")
	f2()
	fmt.Println("Finish f1")
}

func f2() {
	defer fmt.Println("Defer in f2")
	fmt.Println("Start f2")
	fmt.Println("Finish f2")
}
```

**Output**

```
Stat main
Start f1
Start f2
Finish f2
Defer in f2
Finish f1
Defer in f1
Finish main
Defer in main
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*