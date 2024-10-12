<!--yml
category: 未分类
date: 2024-10-13 06:15:32
-->

# Calculate x^y – Pow() in Go (Golang)

> 来源：[https://golangbyexample.com/power-golang/](https://golangbyexample.com/power-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**math** package of GO provides a **Pow** method that can be used to calculate x to the power y.

Below is the signature of the function. It takes input as two float arguments and returns a float

```
func Pow(x, y float64) float64
```

The same function can also be used to calculate the square or cube of a number. Just pass the second argument y as 2 in case of **square** and 3 in case of **cube**

# **Code**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    //Power for integers
    res := math.Pow(2, 10)
    fmt.Println(res)

    //Power for float
    res = math.Pow(1.5, 2)
    fmt.Println(res)

    //Anything to power 0 is 1
    res = math.Pow(3, 0)
    fmt.Println(res)
}
```

**Output:**

```
1024
2.25
1
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [power](https://golangbyexample.com/tag/power/)*