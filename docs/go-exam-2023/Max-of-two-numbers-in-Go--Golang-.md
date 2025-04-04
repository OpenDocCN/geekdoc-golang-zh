<!--yml

类别：未分类

日期：2024-10-13 06:15:48

-->

# Go（Golang）中两个数字的最大值

> 来源：[`golangbyexample.com/max-of-two-numbers-go/`](https://golangbyexample.com/max-of-two-numbers-go/)

目录

+   概述

+   代码：

# **概述**

GO 的**math**包提供了一个**Max**方法，可以用来获取两个数字的最大值。

以下是函数的签名。它接收两个浮点数作为输入，并返回一个浮点数。

```go
func Max(x, y float64) float64
```

另外，**Max**函数的一些特殊情况是

+   Max(x, +Inf) = Max(+Inf, x) = +Inf

+   Max(x, NaN) = Max(NaN, x) = NaN

+   Max(+0, ±0) = Max(±0, +0) = +0

+   Max(-0, -0) = -0

# **代码：**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    max := math.Max(2, 3)
    fmt.Println(max)

    max = math.Max(-2.1, -3.3)
    fmt.Println(max)
}
```

**输出：**

```go
3
-2.1
```


