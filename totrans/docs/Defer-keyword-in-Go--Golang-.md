<!--yml
category: 未分类
date: 2024-10-13 06:25:41
-->

# Defer keyword in Go (Golang)

> 来源：[https://golangbyexample.com/defer-golang/](https://golangbyexample.com/defer-golang/)

This is the  chapter 14 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Pointer](https://golangbyexample.com/pointer-golang/)
**Previous Tutorial** – [Switch](https://golangbyexample.com/switch-statement-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Custom Function in defer](#Custom_Function_in_defer "Custom Function in defer")
*   [Inline Function in Defer](#Inline_Function_in_Defer "Inline Function in Defer")
*   [How does defer works](#How_does_defer_works "How does defer works")
*   [Evaluation of defer arguments](#Evaluation_of_defer_arguments "Evaluation of defer arguments")
*   [Multiple defer functions in the same function](#Multiple_defer_functions_in_the_same_function "Multiple defer functions in the same function")
*   [Defer function and Named Return Values](#Defer_function_and_Named_Return_Values "Defer function and Named Return Values")
*   [Defer and Methods](#Defer_and_Methods "Defer and Methods")
*   [Defer and Panic](#Defer_and_Panic "Defer and Panic")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Defer as the name suggests is used to defer the cleanup activities in a function. These cleanup activities will be performed at the end of the function. This cleanup activities will be done in a different function called by defer.  This different function is called at the end of the surrounding function before it returns. Below is the syntax of defer function

```
defer {function_or_method_call}
```

Things to note about defer function

*   Execution of a deferred function is delayed to the moment the surrounding function returns
*   deferred function will also be executed if the enclosing function terminates abruptly. For example in case of a panic

One good example of understanding the **defer** function is to look at the use case of writing to a file. A file that is opened for writing also must be closed.   

```
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    file.Close()
    return nil
}
```

In the above program, in the **writeToTempFile** function, we are opening a file and then trying to write some content to the file. After we have written the contents of the file we close the file. It is possible that during the write operation it might result into an error and function will return without closing the file. **Defer** function helps to avoid these kinds of problems. **Defer** function is always executed before the surrounding function returns. Let’s rewrite the above program with a **defer** function here.

```
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    defer file.Close()

    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    return nil
}
```

In the above program, we do **defer file.Close()** after opening the file. This will make sure that closing of the file is executed even if the write to the file results into an error. Defer function makes sure that the file will be closed regardless of number of return statements in the function

# **Custom Function in defer**

We can also call a custom function in **defer**. Let’s see an example for that

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

The above function also shows that it is perfectly ok to use defer in the main function as well.

# **Inline Function in Defer**

It is also possible to have an inline function with defer. Let’s see an example of that.

```
package main

import "fmt"

func main() {
    defer func() { fmt.Println("In inline defer") }()
    fmt.Println("Executed")
}
```

**Output**

```
Executed
In inline defer
```

In the above code we have **defer** with a inline function

```
defer func() { fmt.Println("In inline defer") }()
```

This is allowed in go. Also note that it is mandatory to add **“()”** after the function otherwise compiler will raise error

```
expression in defer must be function call
```

As seen from the output, that the inline function is called after everything in the main is executed and before main returns. That is why

```
Executed in main
```

is printed before

```
In inline Defer
```

# **How does defer works**

When the the compiler encounter a defer statement in a function it pushes it onto a list. This list internally implements a stack data structure.  All the encountered defer statement in the same function are pushed onto this list. When the surrounding function returns then all the functions in the stack starting from top to bottom are executed before execution can begin in the calling function. Now same thing will happen in the calling function as well.

Let’s understand what happens when we have multiple defer functions in different functions. Imagine a function call from **main** function to **f1** function to **f2** function

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

# **Evaluation of defer arguments**

defer arguments are evaluated at the time defer statement is evaluated

Let’s see a program for that

```
package main

import "fmt"

func main() {
	sample := "abc"

	defer fmt.Printf("In defer sample is: %s\n", sample)
	sample = "xyz"
}
```

**Output**

```
In defer sample is: abc
```

In the above program when the defer statement was evaluated the value of the **sample** variable was **“abc”**. In the defer function, we print the  sample variable. After the defer statement we change the value of the **sample** variable to **“xyz”**.  But the program outputs **“abc”** instead of **“xyz”** because when the defer arguments were evaluated the value of the  sample variable was **“abc”**.

# **Multiple defer functions in the same function**

 In case we have multiple defer functions within a particular function, then all the  defer functions will be executed in last in first out order

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

# **Defer function and Named Return Values**

In case of named return value in the function, the defer function can read as well as modified those named return values. If the defer function modifies the name return value then that modified value will  be returned

Let’s see a program for that

```
package main
import "fmt"
func main() {
    s := test()
    fmt.Println(s)
}
func test() (size int) {
    defer func() { size = 20 }()
    size = 30
    return
}
```

**Output**

```
20
```

In the above program we have named return value **“size”**  in the the test function. In the defer function we modify the name return value and we change the value to 20.  We then set size to 30. In the main function we print the return value of test function  and it outputs 20 instead of 30 because defer function has modified the value of size variable in the test function

# **Defer and Methods**

**defer** statement is also applicable  for methods in a similar way it is applicable to functions. In the first example we had already seen the **Close** method which was called on the file instance. That shows that the  **defer** statement is also applicable for methods as well

# **Defer and Panic**

defer function will also be executed even if panic happens in a program.  When the panic is raised in a function then the execution of that function is stopped and any deferred function will be executed. In fact deferred function of all the function calls in the stack will also be executed until all the functions have returned .At that time the program will exit and it will print the panic message

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

# **Conclusion**

That is all about defer in golang. Hope you have liked the article. please share feedback/improvements/mistakes in comments

**Next Tutorial** – [Pointer](https://golangbyexample.com/pointer-golang/)
**Previous Tutorial** – [Switch](https://golangbyexample.com/switch-statement-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*