<!--yml

分类：未分类

日期：2024-10-13 06:14:51

-->

# 在 Go (Golang)中去除浮点数的小数点

> 来源：[`golangbyexample.com/remove-decimal-float-go/`](https://golangbyexample.com/remove-decimal-float-go/)

目录

+   概述

+   代码：

# **概述**

**math**包提供了一个**Trunc**方法，可用于移除浮点数的小数点并将其转换为整数

以下是该函数的签名。它接受一个浮点数作为输入，并返回一个浮点数。

```go
func Trunc(x float64) float64\
```

# **代码：**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Trunc(1.6)
    fmt.Println(res)

    res = math.Trunc(-1.6)
    fmt.Println(res)

    res = math.Trunc(1)
    fmt.Println(res)
}
```

**输出：**

```go
1
-1
1
```


