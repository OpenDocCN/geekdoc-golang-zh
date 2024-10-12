<!--yml
category: 未分类
date: 2024-10-13 06:27:28
-->

# Can defer be used inside main function in Go (Golang)

> 来源：[https://golangbyexample.com/defer-inside-main-golang/](https://golangbyexample.com/defer-inside-main-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Defer as the name suggests is used to defer the cleanup activities in a function. These cleanup activities will be performed at the end of the function.

defer can be used inside the main function as well. Let’s see an example for that

# **Example**

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

The above function also that it is perfectly ok to use defer in the main function as well.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*