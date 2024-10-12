<!--yml
category: 未分类
date: 2024-10-13 06:14:45
-->

# Round Even a number in Go (Golang)

> 来源：[https://golangbyexample.com/round-even-number-golang/](https://golangbyexample.com/round-even-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **RoundToEven** method that can be used to round to even a number. It returns the nearest integer rounding ties to even.

Here you can read about use case of RoundToEven – [https://mathematica.stackexchange.com/questions/2116/why-round-to-even-integers/2120](https://mathematica.stackexchange.com/questions/2116/why-round-to-even-integers/2120)

Below is the signature of the function. It takes input a float and also returns a float.

```
func RoundToEven(x float64) float64
```

Some special cases of RoundToEven function are

*   RoundToEven(±0) = ±0
*   RoundToEven(±Inf) = ±Inf
*   RoundToEven(NaN) = NaN

# **Code:**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.RoundToEven(0.5)
    fmt.Println(res)

    res = math.RoundToEven(1.5)
    fmt.Println(res)

    res = math.RoundToEven(2.5)
    fmt.Println(res)

    res = math.RoundToEven(3.5)
    fmt.Println(res)

    res = math.RoundToEven(4.5)
    fmt.Println(res)
}
```

**Output:**

```
0
2
2
4
4
```

*   [go](https://golangbyexample.com/tag/go/)*   [math](https://golangbyexample.com/tag/math/)*   [round even](https://golangbyexample.com/tag/round-even/)*