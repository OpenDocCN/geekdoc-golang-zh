<!--yml

category: 未分类

date: 2024-10-13 06:29:05

-->

# Go（Golang）中的指针默认零值

> 来源：[`golangbyexample.com/default-zero-value-pointer-golang/`](https://golangbyexample.com/default-zero-value-pointer-golang/)

目录

+   概述

+   程序

# **概述**

指针的默认零值是 nil。让我们看看一个程序

# **程序**

```go
package main

import "fmt"

func main() {
    var a *int
    fmt.Print("Default Zero Value of a pointer: ")
    fmt.Println(a)
}
```

**输出：**

```go
Default value of pointer: 
```


