<!--yml

分类：未分类

日期：2024-10-13 06:15:06

-->

# 在 Go (Golang) 中的数字立方根

> 来源：[https://golangbyexample.com/cube-root-number-golang/](https://golangbyexample.com/cube-root-number-golang/)

目录

**   [概述](#Overview "概述")

+   [代码：](#Code "代码：")*  *# **概述**

GO 的 **math** 包提供了一个 **Cbrt** 方法，可以用于获取该数字的立方根。

以下是该函数的签名。它接受一个浮点数作为输入，并返回一个浮点数。

```go
func Cbrt(x float64) float64
```

Cbrt 函数的一些特殊情况是

+   Cbrt(±0) = ±0

+   Cbrt(±Inf) = ±Inf

+   Cbrt(NaN) = NaN

# **代码：**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Cbrt(8)
    fmt.Println(res)

    res = math.Cbrt(27)
    fmt.Println(res)

    res = math.Cbrt(30.33)
    fmt.Println(res)
}
```

**输出：**

```go
2
3
3.118584170228812
```

+   [立方根](https://golangbyexample.com/tag/cube-root/)*   [go](https://golangbyexample.com/tag/go/)*   [数字](https://golangbyexample.com/tag/number/)*
