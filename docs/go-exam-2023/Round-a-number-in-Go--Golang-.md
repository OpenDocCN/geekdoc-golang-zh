<!--yml

分类：未分类

日期：2024-10-13 06:14:40

-->

# 在 Go（Golang）中四舍五入一个数字

> 来源：[`golangbyexample.com/round-number-golang/`](https://golangbyexample.com/round-number-golang/)

目录

+   概述

+   代码：

# **概述**

Go 的**math**包提供了一个可以用于四舍五入数字的**Round**方法。它返回最接近的整数值。

以下是该函数的签名。它接受一个浮点数作为输入，并返回一个浮点数。

```go
func Round(x float64) float64
```

Round 函数的一些特殊情况是

+   Round(±0) = ±0

+   Round(±Inf) = ±Inf

+   Round(NaN) = NaN

# **代码：**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Round(1.6)
    fmt.Println(res)

    res = math.Round(1.5)
    fmt.Println(res)

    res = math.Round(1.4)
    fmt.Println(res)

    res = math.Round(-1.6)
    fmt.Println(res)

    res = math.Round(-1.5)
    fmt.Println(res)

    res = math.Round(-1.4)
    fmt.Println(res)

    res = math.Round(1)
    fmt.Println(res)
}
```

**输出：**

```go
2
2
1
-2
-2
-1
1
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [round](https://golangbyexample.com/tag/round/)*
