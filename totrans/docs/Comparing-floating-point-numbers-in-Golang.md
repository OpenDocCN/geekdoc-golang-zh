<!--yml
category: 未分类
date: 2024-10-13 06:05:52
-->

# Comparing floating point numbers in Golang

> 来源：[https://golangbyexample.com/comparing-floating-point-numbers-go/](https://golangbyexample.com/comparing-floating-point-numbers-go/)

Table of Contents

 **   [Introduction](#Introduction "Introduction")
*   [Comparing Same Float Types](#Comparing_Same_Float_Types "Comparing Same Float Types")
*   [Comparing Different Float Types](#Comparing_Different_Float_Types "Comparing Different Float Types")
*   [Comparing Same Float Types With Tolerance](#Comparing_Same_Float_Types_With_Tolerance "Comparing Same Float Types With Tolerance")
*   [Comparing Using math.big package](#Comparing_Using_mathbig_package "Comparing Using math.big package")*  *# Introduction

Go Lang has two types of floats

*   float32
*   float64

# **Comparing Same Float Types**

Two floating point numbers can be compared using  Go’s == operator assuming they are the same float types. See below example

```
package main

import "fmt"

func main() {
    a := 3.14
    b := 3.14
    if a == b {
        fmt.Println("Same")
    } else {
        fmt.Println("Not Same")
    }

    a = 3.142
    b = 3.14
    if a == b {
        fmt.Println("Same")
    } else {
        fmt.Println("Not Same")
    }
}
```

**Output:**

```
Same
Not Same
```

# **Comparing Different Float Types**

Comparison of float32 with float64 will give a compilation error

```
package main

import "fmt"

func main() {

    var a float32
    var b float64
    if a == b {
        fmt.Println("Same")
    } else {
        fmt.Println("Not Same")
    }
}
```

**Output:**

```
invalid operation: a == b (mismatched types float32 and float64)
```

# **Comparing Same Float Types With Tolerance**

If some kind of tolerance is acceptable while comparing float types, then below approach can be used

```
package main

import (
    "fmt"
    "math"
)

func main() {
    withTolerane(3.14, 3.141)
    withTolerane(3.14, 3.142)
}

func withTolerane(a, b float64) {
    tolerance := 0.001
    if diff := math.Abs(a - b); diff < tolerance {
        fmt.Printf("When a=%f and b =%f => Nearly same by tolerance\n", a, b)
    } else {
        fmt.Printf("When a=%f and b=%f => Not same Even by Tolerance\n", a, b)
    }
}
```

**Output:**

```
When a=3.140000 and b =3.141000 => Nearly same by tolerance
When a=3.140000 and b=3.142000 => Not same Even by Tolerance
```

# **Comparing Using math.big package**

The big package supports big numbers and it supports Int, Rational Number and Float. It has a compare method which can be used to compare two floats

```
package main

import (
    "fmt"
    "math/big"
)

func main() {
    a := 3.1432
    b := 3.1456
    // compare a to b
    result := big.NewFloat(a).Cmp(big.NewFloat(b))

    // -1 if a < b
    if result < 0 {
        fmt.Println("a less than b")
    }

    // 0 if a == b
    if result == 0 {
        fmt.Println("a  equals to b")
    }

    // +1 if a > b
    if result > 0 {
        fmt.Println("a 1 more than b")
    }
}
```

**Output:**

```
a less than b
```

*   [compare](https://golangbyexample.com/tag/compare/)*   [float](https://golangbyexample.com/tag/float/)*   [floating point numbers in go](https://golangbyexample.com/tag/floating-point-numbers-in-go/)*