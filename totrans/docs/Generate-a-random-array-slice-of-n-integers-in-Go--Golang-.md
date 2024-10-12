<!--yml
category: 未分类
date: 2024-10-13 06:16:35
-->

# Generate a random array/slice of n integers in Go (Golang)

> 来源：[https://golangbyexample.com/generate-random-array-slice-golang/](https://golangbyexample.com/generate-random-array-slice-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math/rand** package of GO provides a **Perm** method that can be used generate the pseudo-random slice of n integers. The array will be pseudo-random permutation of the integers in the range [0,n).

To know more about what pseudo-random number means, checkout this post – [https://golangbyexample.com/generate-random-number-golang](https://golangbyexample.com/generate-random-number-golang)
Below is the signature of the function. It takes a number n as input and returns the permuted slice.

```
func Perm(n int) []int
```

# **Code:**

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    //Provide seed
    rand.Seed(time.Now().Unix())

    //Generate a random array of length n
    fmt.Println(rand.Perm(10))
    fmt.Println(rand.Perm(10))
}
```

**Output:**

```
[6 0 1 5 9 4 2 3 7 8]
[9 8 5 0 3 4 6 7 2 1]
```

*   [array](https://golangbyexample.com/tag/array/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [random](https://golangbyexample.com/tag/random/)*   [slice](https://golangbyexample.com/tag/slice/)*