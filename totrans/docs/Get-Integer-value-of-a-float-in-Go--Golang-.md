<!--yml
category: 未分类
date: 2024-10-13 06:14:35
-->

# Get Integer value of a float in Go (Golang)

> 来源：[https://golangbyexample.com/integer-value-of-float/](https://golangbyexample.com/integer-value-of-float/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**math** package of GO provides a **Trunc** method that can be used to get integer value of a float

Below is the signature of the function. It takes input a float and also returns a float.

```
func Trunc(x float64) float64
```

Some special cases of **Trunc** function are

*   Trunc(±0) = ±0
*   Trunc(±Inf) = ±Inf
*   Trunc(NaN) = NaN

# **Code**

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

**Output:**

```
1
-1
1
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [integer value of float](https://golangbyexample.com/tag/integer-value-of-float/)*   [trunc](https://golangbyexample.com/tag/trunc/)*