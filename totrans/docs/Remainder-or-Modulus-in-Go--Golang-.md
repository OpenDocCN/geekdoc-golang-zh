<!--yml
category: 未分类
date: 2024-10-13 06:15:22
-->

# Remainder or Modulus in Go (Golang)

> 来源：[https://golangbyexample.com/remainder-modulus-go-golang/](https://golangbyexample.com/remainder-modulus-go-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [% Operator](#_Operator "% Operator")
    *   [Code](#Code "Code")
*   [Mod function for floats](#Mod_function_for_floats "Mod function for floats")
    *   [Code](#Code-2 "Code")
*   [IEEE 754 Remainder](#IEEE_754_Remainder "IEEE 754 Remainder")
    *   [Code](#Code-3 "Code")*  *# **Overview**

In this tutorial we will study about

*   **% Operator** – applicable to get remainder for integers

*   **Mod function** – can be used to get remainder in case of floats also

*   **Remainder function** – can be used to get IEEE 754 remainder

# **% Operator**

Golang has a modulus operator **(‘ %’),** that can be used to get the remainder on dividing two integer numbers. Let’s see a working program illustrating this

## **Code**

```
package main

import (
    "fmt"
)

func main() {
    res := 4 % 2
    fmt.Println(res)

    res = 5 % 2
    fmt.Println(res)

    res = 8 % 3
    fmt.Println(res)
}
```

**Output:**

```
0
1
2
```

# **Mod function for floats**

% function doesn’t work for floats. For getting remainder when dividing two floats we can use the **Mod** function provided by the **math** package itself. Below is the signature of the function. It takes in two floats and returns a float. It will return floating point remainder of x/y. The output will take the sign on x

```
func Mod(x, y float64) float64
```

Some special cases of **Mod** function are

*   Mod(±Inf, y) = NaN
*   Mod(NaN, y) = NaN
*   Mod(x, 0) = NaN
*   Mod(x, ±Inf) = x
*   Mod(x, NaN) = NaN

## **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Mod(4, 2)
    fmt.Println(res)

    res = math.Mod(4.2, 2)
    fmt.Println(res)

    res = math.Mod(5, 2)
    fmt.Println(res)

    res = math.Mod(-5, 2)
    fmt.Println(res)
}
```

**Output**

```
0
0.20000000000000018
1
-1
```

# **IEEE 754 Remainder**

**math** package of GO provides a **Remainder** method that can be used to get **IEEE 754 Remainder** of two numbers, one acting as a Numerator and the acting as a denominator.

You can read more about why do we need **IEEE 754 Remainder** here – [https://stackoverflow.com/questions/26671975/why-do-we-need-ieee-754-remainder](https://stackoverflow.com/questions/26671975/why-do-we-need-ieee-754-remainder)

Below is the signature of the function. It takes input two numbers as float64 and returns a remainder which is also an **IEEE 754 float64 Remainder**.

```
func Remainder(x, y float64) float64
```

Some special cases of Remainder function are

*   Remainder(±Inf, y) = NaN
*   Remainder(NaN, y) = NaN
*   Remainder(x, 0) = NaN
*   Remainder(x, ±Inf) = x
*   Remainder(x, NaN) = NaN

## **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Remainder(4, 2)
    fmt.Println(res)

    res = math.Remainder(5, 2)
    fmt.Println(res)

    res = math.Remainder(5.5, 2)
    fmt.Println(res)

    res = math.Remainder(5.5, 1.5)
    fmt.Println(res)
}
```

**Output**

```
0
1
-0.5
-0.5
```

*   [go](https://golangbyexample.com/tag/go/)*   [math](https://golangbyexample.com/tag/math/)*   [mod](https://golangbyexample.com/tag/mod/)*   [modulus](https://golangbyexample.com/tag/modulus/)*