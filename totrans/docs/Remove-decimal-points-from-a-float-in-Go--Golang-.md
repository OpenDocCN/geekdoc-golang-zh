<!--yml
category: 未分类
date: 2024-10-13 06:14:51
-->

# Remove decimal points from a float in Go (Golang)

> 来源：[https://golangbyexample.com/remove-decimal-float-go/](https://golangbyexample.com/remove-decimal-float-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **Trunc** method that can be used to remove decimal points of a float and convert it to an integer

Below is the signature of the function. It takes input a float and also returns a float.

```
func Trunc(x float64) float64\
```

# **Code:**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Trunc(1.6)
    fmt.Println(res)

    res = math.Trunc(-1.6)
    fmt.Println(res)

    res = math.Trunc(1)
    fmt.Println(res)
}
```

**Output:**

```
1
-1
1
```

*   [go](https://golangbyexample.com/tag/go/)*   [math](https://golangbyexample.com/tag/math/)*