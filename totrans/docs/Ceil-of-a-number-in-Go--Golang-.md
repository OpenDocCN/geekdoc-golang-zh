<!--yml
category: 未分类
date: 2024-10-13 06:14:22
-->

# Ceil of a number in Go (Golang)

> 来源：[https://golangbyexample.com/ceil-number-golang/](https://golangbyexample.com/ceil-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **Ceil** method that can be used to get the ceil of a number. Ceil of a number is the least integer value greater than or equal to that number.

Below is the signature of the function. It takes input a float and also returns a float.

```
func Ceil(x float64) float64
```

Some special cases of ceil function are

*   Ceil(±0) = ±0
*   Ceil(±Inf) = ±Inf
*   Ceil(NaN) = NaN

# **Code:**

```
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

**Output:**

```
2
-1
1
```

*   [ceil](https://golangbyexample.com/tag/ceil/)*   [golang](https://golangbyexample.com/tag/golang/)*