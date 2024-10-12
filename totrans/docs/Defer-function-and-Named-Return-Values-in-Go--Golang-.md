<!--yml
category: 未分类
date: 2024-10-13 06:27:19
-->

# Defer function and Named Return Values in Go (Golang)

> 来源：[https://golangbyexample.com/defer-named-return-values-golang/](https://golangbyexample.com/defer-named-return-values-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

In case of named return value in the function, the defer function can read as well as modified those named return values. If the defer function modifies the name return value then that modified value will  be returned

Let’s see a program for that

# **Example**

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*