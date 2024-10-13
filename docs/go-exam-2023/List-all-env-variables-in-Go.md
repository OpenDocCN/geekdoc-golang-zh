<!--yml

分类：未分类

日期：2024-10-13 06:09:33

-->

# 列出 Go 中的所有环境变量

> 来源：[`golangbyexample.com/list-all-env-variables-go/`](https://golangbyexample.com/list-all-env-variables-go/)

目录

+   概述

+   代码：

# **概述**

**os** 包提供了一个 **Environ()** 函数，用于获取所有环境变量的列表。

+   获取所有环境变量。它返回一个字符串数组。

```go
func Environ() []string 
```

还有一种方法可以清除所有环境变量。**Clearenv()**函数可以用来实现同样的功能。

```go
func Clearenv()
```

# **代码：**

```go
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    //Set env a to b
    err := os.Setenv("a", "b")
    if err != nil {
        log.Fatal(err)
    }

    err = os.Setenv("c", "d")
    if err != nil {
        log.Fatal(err)
    }

    //Get all env variables
    fmt.Println(os.Environ())

    //Clear all env variables
    os.Clearenv()

    //Again get all env variable
    fmt.Println(os.Environ())
}
```

**输出：**

```go
List of all env varialbes on your system including a and b
```*
