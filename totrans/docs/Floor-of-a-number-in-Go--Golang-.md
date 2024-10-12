<!--yml
category: 未分类
date: 2024-10-13 06:14:27
-->

# Floor of a number in Go (Golang)

> 来源：[https://golangbyexample.com/floor-number-golang/](https://golangbyexample.com/floor-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of go provides a **Floor** method that can be used to get the ceil of a number. Floor of a number is the greatest integer value less than or equal to that number.

Below is the signature of the function. It takes input a float and also returns a float.

```
func Floor(x float64) float64
```

Some special cases of floor function are

*   Floor(±0) = ±0
*   Floor(±Inf) = ±Inf
*   Floor(NaN) = NaN

# **Code:**

```
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

**Output:**

```
1
-2
1
```

*   [go](https://golangbyexample.com/tag/go/)*   [number](https://golangbyexample.com/tag/number/)*