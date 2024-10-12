<!--yml
category: 未分类
date: 2024-10-13 06:16:09
-->

# Pick a random element in an array or slice in Go (Golang)

> 来源：[https://golangbyexample.com/pick-random-element-array-slice-go/](https://golangbyexample.com/pick-random-element-array-slice-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**‘mat/rand’** package of golang contains a **Intn** function that can be used to generate a pseudo-random number between [0,n). Bracket at the end means that n is exclusive. This function can be utilized to pick a random element in an array or slice of int or string.

To know more about what pseudo-random number means, checkout this post – [https://golangbyexample.com/generate-random-number-golang](https://golangbyexample.com/generate-random-number-golang)

Below is the signature of this method. It takes input a number n and will return a number x in range 0<=x<n.

```
func Intn(n int) int
```

# **Code**

We can directly index an element in a slice of int. See below program for picking up random from a slice of int.

```
package main

import (
    "fmt"
    "math/rand"
)

func main() {
    in := []int{2, 5, 6}
    randomIndex := rand.Intn(len(in))
    pick := in[randomIndex]
    fmt.Println(pick)
}
```

**Output:**

```
Between 2, 5 or 6
```

*   [array](https://golangbyexample.com/tag/array/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [pick](https://golangbyexample.com/tag/pick/)*   [slice](https://golangbyexample.com/tag/slice/)*