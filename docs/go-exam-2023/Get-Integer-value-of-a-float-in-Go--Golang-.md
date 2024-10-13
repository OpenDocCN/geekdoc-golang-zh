<!--yml

类别：未分类

日期：2024-10-13 06:14:35

-->

# 在 Go (Golang) 中获取浮点数的整数值

> 来源：[https://golangbyexample.com/integer-value-of-float/](https://golangbyexample.com/integer-value-of-float/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# **概述**

GO 的 **math** 包提供了一个 **Trunc** 方法，可以用来获取浮点数的整数值

以下是该函数的签名。它接受一个浮点数作为输入，并返回一个浮点数。

```
func Trunc(x float64) float64
```

**Trunc** 函数的一些特殊情况是

+   Trunc(±0) = ±0

+   Trunc(±Inf) = ±Inf

+   Trunc(NaN) = NaN

# **代码**

```
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

```
1
-1
1
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [浮点数的整数值](https://golangbyexample.com/tag/integer-value-of-float/)*   [trunc](https://golangbyexample.com/tag/trunc/)*
