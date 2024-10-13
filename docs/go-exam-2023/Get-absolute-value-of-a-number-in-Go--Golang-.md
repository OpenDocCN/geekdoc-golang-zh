<!--yml

类别：未分类

日期：2024-10-13 06:14:56

-->

# 在 Go (Golang) 中获取数字的绝对值

> 来源：[https://golangbyexample.com/absolute-value-number-golang/](https://golangbyexample.com/absolute-value-number-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

GO 的 **math** 包提供了一个 **Abs** 方法，可以用来获取数字的绝对值。

以下是该函数的签名。它接受一个浮点数作为输入，并返回一个浮点数。

```go
func Abs(x float64) float64
```

Abs 函数的一些特殊情况是

+   Abs(±0) = ±0

+   Abs(±Inf) = ±Inf

+   Abs(NaN) = NaN

# **代码：**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Abs(-2)
    fmt.Println(res)

    res = math.Abs(2)
    fmt.Println(res)

    res = math.Abs(3.5)
    fmt.Println(res)

    res = math.Abs(-3.5)
    fmt.Println(res)
}
```

**输出：**

```go
2
2
3.5
3.5
```

+   [去](https://golangbyexample.com/tag/go/)*   [数学](https://golangbyexample.com/tag/math/)*   [数字](https://golangbyexample.com/tag/number/)*
