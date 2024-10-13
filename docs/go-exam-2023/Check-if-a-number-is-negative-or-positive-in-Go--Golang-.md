<!--yml

类别：未分类

日期：2024-10-13 06:15:37

-->

# 在 Go (Golang)中检查一个数字是负数还是正数

> 来源：[`golangbyexample.com/num-positive-negative-go/`](https://golangbyexample.com/num-positive-negative-go/)

目录

+   概述

+   代码

# **概述**

**math**包提供了一个**Signbit**方法，可以用来检查给定的数字是负数还是正数。

+   对于负数，它返回 true

+   对于正数，它返回 false

以下是该函数的签名。它接收一个浮点数作为输入，并返回一个布尔值

```go
func Signbit(x float64) bool 
```

# **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    //Will return false for positive
    res := math.Signbit(4)
    fmt.Println(res)

    //Will return true for negative
    res = math.Signbit(-4)
    fmt.Println(res)

    //Will return false for zerp
    res = math.Signbit(0)
    fmt.Println(res)

    //Will return false for positive float
    res = math.Signbit(-0)
    fmt.Println(res)
}
```

**输出：**

```go
false
true
false
false
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [number](https://golangbyexample.com/tag/number/)*
