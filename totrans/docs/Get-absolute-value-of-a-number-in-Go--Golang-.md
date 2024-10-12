<!--yml
category: 未分类
date: 2024-10-13 06:14:56
-->

# Get absolute value of a number in Go (Golang)

> 来源：[https://golangbyexample.com/absolute-value-number-golang/](https://golangbyexample.com/absolute-value-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides an **Abs** method that can be used to get the absolute value of a number.

Below is the signature of the function. It takes input a float and also returns a float.

```
func Abs(x float64) float64
```

Some special cases of Abs function are

*   Abs(±0) = ±0
*   Abs(±Inf) = ±Inf
*   Abs(NaN) = NaN

# **Code:**

```
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

**Output:**

```
2
2
3.5
3.5
```

*   [go](https://golangbyexample.com/tag/go/)*   [math](https://golangbyexample.com/tag/math/)*   [number](https://golangbyexample.com/tag/number/)*