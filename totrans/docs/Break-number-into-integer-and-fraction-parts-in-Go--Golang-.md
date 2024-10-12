<!--yml
category: 未分类
date: 2024-10-13 06:15:27
-->

# Break number into integer and fraction parts in Go (Golang)

> 来源：[https://golangbyexample.com/break-integer-fraction-part-go/](https://golangbyexample.com/break-integer-fraction-part-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**math** package of GO provides a **Modf** method that can be used to break a floating-point number into integer and floating part. Please note that the integer part is also returned as a float by this function.

Below is the signature of the function. It takes input as a float number and returns two float64\. The first one is the integer part and second is the fractional part.

```
func Modf(f float64) (int float64, frac float64)
```

Some points to note about above function

*   The return values **int** and **frac** add up to input **f**
*   **int** and **frac** has same sign a input **f**

Also some special cases of **Modf** function are

*   Modf(±Inf) = ±Inf, NaN
*   Modf(NaN) = NaN, NaN

# **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    //Contain both integer and fraction
    integer, fraction := math.Modf(2.5)
    fmt.Printf("Integer: %f. Fraction: %f\n", integer, fraction)

    //Contain only integer part
    integer, fraction = math.Modf(2)
    fmt.Printf("Integer: %f. Fraction: %f\n", integer, fraction)

    //Negative floating point number
    integer, fraction = math.Modf(-2.5)
    fmt.Printf("Integer: %f. Fraction: %f\n", integer, fraction)

    //Contains only fraction part
    integer, fraction = math.Modf(0.5)
    fmt.Printf("Integer: %f. Fraction: %f\n", integer, fraction)
}
```

**Output:**

```
Integer: 2.000000\. Fraction: 0.500000
Integer: 2.000000\. Fraction: 0.000000
Integer: -2.000000\. Fraction: -0.500000
Integer: 0.000000\. Fraction: 0.500000
```

*   [go](https://golangbyexample.com/tag/go/)*   [integer](https://golangbyexample.com/tag/integer/)*   [math](https://golangbyexample.com/tag/math/)*