<!--yml
category: 未分类
date: 2024-10-13 06:15:48
-->

# Max of two numbers in Go (Golang)

> 来源：[https://golangbyexample.com/max-of-two-numbers-go/](https://golangbyexample.com/max-of-two-numbers-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **Max** method that can be used to get the maximum of two numbers.

Below is the signature of the function. It takes input two float numbers and returns a float.

```
func Max(x, y float64) float64
```

Also some special cases of **Max** function are

*   Max(x, +Inf) = Max(+Inf, x) = +Inf
*   Max(x, NaN) = Max(NaN, x) = NaN
*   Max(+0, ±0) = Max(±0, +0) = +0
*   Max(-0, -0) = -0

# **Code:**

```
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

**Output:**

```
3
-2.1
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [max](https://golangbyexample.com/tag/max/)*   [maximum](https://golangbyexample.com/tag/maximum/)*