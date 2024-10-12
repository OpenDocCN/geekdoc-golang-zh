<!--yml
category: 未分类
date: 2024-10-13 06:25:20
-->

# Ignoring errors in Go (Golang)

> 来源：[https://golangbyexample.com/ignoring-errors-golang/](https://golangbyexample.com/ignoring-errors-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

Underscore (‘_’) operator can be used to ignore the error returned from a function call.  Before we see a program it’s important to note that error should never be ignored. It is not a recommended way. Let’s see a program

# **Code**

```
package main
import (
    "fmt"
    "os"
)
func main() {
    file, _ := os.Open("non-existing.txt")
    fmt.Println(file)
}
```

**Output**

```
{nil}
```

In the above program, we used the underscore operator to ignore the error while opening a non-existing file. That is why the file instance returned by the function is nil. Therefore it is better to check for an error before using any other argument returned by the function as that can be nil and would result in unwanted issues and also sometimes it might also result in a panic.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*