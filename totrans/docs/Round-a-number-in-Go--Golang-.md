<!--yml
category: 未分类
date: 2024-10-13 06:14:40
-->

# Round a number in Go (Golang)

> 来源：[https://golangbyexample.com/round-number-golang/](https://golangbyexample.com/round-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **Round** method that can be used to round a number. It returns the nearest integer value.

Below is the signature of the function. It takes input a float and also returns a float.

```
func Round(x float64) float64
```

Some special cases of Round function are

*   Round(±0) = ±0
*   Round(±Inf) = ±Inf
*   Round(NaN) = NaN

# **Code:**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Round(1.6)
    fmt.Println(res)

    res = math.Round(1.5)
    fmt.Println(res)

    res = math.Round(1.4)
    fmt.Println(res)

    res = math.Round(-1.6)
    fmt.Println(res)

    res = math.Round(-1.5)
    fmt.Println(res)

    res = math.Round(-1.4)
    fmt.Println(res)

    res = math.Round(1)
    fmt.Println(res)
}
```

**Output:**

```
2
2
1
-2
-2
-1
1
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [round](https://golangbyexample.com/tag/round/)*