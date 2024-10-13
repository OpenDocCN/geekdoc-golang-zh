<!--yml

category: 未分类

date: 2024-10-13 06:14:27

-->

# Go（Golang）中的数字下限

> 来源：[`golangbyexample.com/floor-number-golang/`](https://golangbyexample.com/floor-number-golang/)

目录

+   概述

+   代码：

# **概述**

**Go**的**math**包提供了一个**Floor**方法，可以用来获取一个数的下限。一个数的下限是小于或等于该数的最大整数值。

下面是该函数的签名。它接受一个浮点数作为输入，并返回一个浮点数。

```go
func Floor(x float64) float64
```

下限函数的一些特殊情况是

+   Floor(±0) = ±0

+   Floor(±Inf) = ±Inf

+   Floor(NaN) = NaN

# **代码：**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Floor(1.6)
    fmt.Println(res)

    res = math.Floor(-1.6)
	fmt.Println(res)

    res = math.Floor(1)
    fmt.Println(res)
}
```

**输出：**

```go
1
-2
1
```

+   [go](https://golangbyexample.com/tag/go/)*   [number](https://golangbyexample.com/tag/number/)*
