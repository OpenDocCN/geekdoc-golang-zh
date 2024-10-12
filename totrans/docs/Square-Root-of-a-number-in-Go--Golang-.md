<!--yml
category: 未分类
date: 2024-10-13 06:15:11
-->

# Square Root of a number in Go (Golang)

> 来源：[https://golangbyexample.com/square-root-number-golang/](https://golangbyexample.com/square-root-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **Sqrt** method that can be used to get the cube root of that number.

Below is the signature of the function. It takes input a float and also returns a float.

```
func Sqrt(x float64) float64
```

Some special cases of Sqrt function are

*   Sqrt(±0) = ±0
*   Sqrt(±Inf) = ±Inf
*   Sqrt(x < 0) = NaN
*   Sqrt(NaN) = NaN

# **Code:**

```
package main

import (
	"fmt"
	"math"
)

func main() {
	//Square root of a integer
	res := math.Sqrt(4)
	fmt.Println(res)

	//Square root of a integer
	res = math.Sqrt(9)
	fmt.Println(res)

	//Square Root of a float
	res = math.Sqrt(30.33)
	fmt.Println(res)

	//Square Root of a negative number
	res = math.Sqrt(-9)
	fmt.Println(res)
} 
```

**Output:**

```
2
3
5.5072679252057455
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [square root](https://golangbyexample.com/tag/square-root/)*