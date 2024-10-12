<!--yml
category: 未分类
date: 2024-10-13 06:15:06
-->

# Cube Root of a number in Go (Golang)

> 来源：[https://golangbyexample.com/cube-root-number-golang/](https://golangbyexample.com/cube-root-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **Cbrt** method that can be used to get the cube root of that number.

Below is the signature of the function. It takes input a float and also returns a float.

```
func Cbrt(x float64) float64
```

Some special cases of Cbrt function are

*   Cbrt(±0) = ±0
*   Cbrt(±Inf) = ±Inf
*   Cbrt(NaN) = NaN

# **Code:**

```
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

**Output:**

```
2
3
3.118584170228812
```

*   [cube root](https://golangbyexample.com/tag/cube-root/)*   [go](https://golangbyexample.com/tag/go/)*   [number](https://golangbyexample.com/tag/number/)*