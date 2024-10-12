<!--yml
category: 未分类
date: 2024-10-13 06:26:58
-->

# Custom Function in defer in Go (Golang)

> 来源：[https://golangbyexample.com/custom-function-defer-golang/](https://golangbyexample.com/custom-function-defer-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

We can also call a custom function in defer. Let’s see a example for that

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

The above function also shows that it is perfectly ok to use defer in the main function as well.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*