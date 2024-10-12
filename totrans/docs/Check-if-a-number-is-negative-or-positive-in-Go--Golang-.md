<!--yml
category: 未分类
date: 2024-10-13 06:15:37
-->

# Check if a number is negative or positive in Go (Golang)

> 来源：[https://golangbyexample.com/num-positive-negative-go/](https://golangbyexample.com/num-positive-negative-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**math** package of GO provides a **Signbit** method that can be used to check whether a given number is negative or positive.

*   It returns true for a negative number
*   It returns false for a positive number

Below is the signature of the function. It takes input a float and returns a bool

```
func Signbit(x float64) bool 
```

# **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    //Will return false for positive
    res := math.Signbit(4)
    fmt.Println(res)

    //Will return true for negative
    res = math.Signbit(-4)
    fmt.Println(res)

    //Will return false for zerp
    res = math.Signbit(0)
    fmt.Println(res)

    //Will return false for positive float
    res = math.Signbit(-0)
    fmt.Println(res)
}
```

**Output:**

```
false
true
false
false
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [number](https://golangbyexample.com/tag/number/)*