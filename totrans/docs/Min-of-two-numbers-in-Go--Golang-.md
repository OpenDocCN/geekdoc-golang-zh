<!--yml
category: 未分类
date: 2024-10-13 06:15:42
-->

# Min of two numbers in Go (Golang)

> 来源：[https://golangbyexample.com/min-of-two-numbers-golang/](https://golangbyexample.com/min-of-two-numbers-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **Min** method that can be used to get the minimum of two numbers. The

Below is the signature of the function. It takes input two float numbers and returns a float.

```
func Min(x, y float64) float64
```

Also some special cases of **Min** function are

*   Min(x, +Inf) = Min(+Inf, x) = +Inf
*   Min(x, NaN) = Min(NaN, x) = NaN
*   Min(+0, ±0) = Min(±0, +0) = +0
*   Min(-0, -0) = -0

# **Code:**

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

**Output:**

```
2
-3.3
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [min](https://golangbyexample.com/tag/min/)*   [minimum](https://golangbyexample.com/tag/minimum/)*