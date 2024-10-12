<!--yml
category: 未分类
date: 2024-10-13 06:29:05
-->

# Default Zero Value of Pointer in Go (Golang)

> 来源：[https://golangbyexample.com/default-zero-value-pointer-golang/](https://golangbyexample.com/default-zero-value-pointer-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Default zero value of a pointer is nil. Let’s see a program for it

# **Program**

```
package main

import "fmt"

func main() {
    var a *int
    fmt.Print("Default Zero Value of a pointer: ")
    fmt.Println(a)
}
```

**Output:**

```
Default value of pointer: 
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*