<!--yml

类别: 未分类

日期: 2024-10-13 06:14:22

-->

# Go (Golang)中的向上取整

> 来源：[https://golangbyexample.com/ceil-number-golang/](https://golangbyexample.com/ceil-number-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码:](#Code "Code:")*  *# **概述**

**math**包提供了一个**Ceil**方法，可以用于获取一个数字的向上取整值。一个数字的向上取整是大于或等于该数字的最小整数值。

以下是该函数的签名。它接受一个浮点数作为输入，并返回一个浮点数。

```go
func Ceil(x float64) float64
```

向上取整函数的一些特殊情况是

+   向上取整(±0) = ±0

+   向上取整(±Inf) = ±Inf

+   向上取整(NaN) = NaN

# **代码:**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Ceil(1.6)
    fmt.Println(res)

    res = math.Ceil(-1.6)
	fmt.Println(res)

    res = math.Ceil(1)
    fmt.Println(res)
}
```

**输出:**

```go
2
-1
1
```

+   [向上取整](https://golangbyexample.com/tag/ceil/)*   [Golang](https://golangbyexample.com/tag/golang/)*
