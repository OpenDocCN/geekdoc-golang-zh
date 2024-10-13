<!--yml

category: 未分类

date: 2024-10-13 06:15:42

-->

# Go（Golang）中的两个数字的最小值

> 来源：[https://golangbyexample.com/min-of-two-numbers-golang/](https://golangbyexample.com/min-of-two-numbers-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

**math** 包提供了一个 **Min** 方法，可以用来获取两个数字的最小值。该

以下是函数的签名。它接受两个浮点数作为输入并返回一个浮点数。

```
func Min(x, y float64) float64
```

另外，一些 **Min** 函数的特殊情况是

+   Min(x, +Inf) = Min(+Inf, x) = +Inf

+   Min(x, NaN) = Min(NaN, x) = NaN

+   Min(+0, ±0) = Min(±0, +0) = +0

+   Min(-0, -0) = -0

# **代码：**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    min := math.Min(2, 3)
    fmt.Println(min)

    min = math.Min(-2.1, -3.3)
    fmt.Println(min)
}
```

**输出：**

```
2
-3.3
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [min](https://golangbyexample.com/tag/min/)*   [minimum](https://golangbyexample.com/tag/minimum/)*
