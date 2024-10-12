<!--yml
category: 未分类
date: 2024-10-13 06:15:01
-->

# Pi value in Go (Golang)

> 来源：[https://golangbyexample.com/pi-value-golang/](https://golangbyexample.com/pi-value-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math** package of GO provides a **Pi** constant.

This is how it is defined in the constant.go file of math package. It is a float64 value

```
Pi  = 3.14159265358979323846264338327950288419716939937510582097494459
```

# **Code:**

```
package main

import (
	"fmt"
	"math"
)

func main() {
	pi := math.Pi
	fmt.Println(pi)
} 
```

**Output:**

```
3.141592653589793
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [pi](https://golangbyexample.com/tag/pi/)*