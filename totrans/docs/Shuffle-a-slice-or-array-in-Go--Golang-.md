<!--yml
category: 未分类
date: 2024-10-13 06:16:25
-->

# Shuffle a slice or array in Go (Golang)

> 来源：[https://golangbyexample.com/shuffle-slice-or-array-go/](https://golangbyexample.com/shuffle-slice-or-array-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**math/rand** package of go provides a **Shuffle** method that can be used shuffle an array or a slice. This method pseudo-randomizes the order of elements using the default source. pseudo-randomizes means that for a fixed input seed it will generate the same randomization. That is why in our program we will initialize the rand package with a different seed every time.

Below is the signature of the function.

```
func Shuffle(n int, swap func(i, j int))
```

This function takes in arguments

*   First is the length of the array or slice.

*   The second is a swap function that will be called for different indexes **i** and **j.** You need to provide your own swap function that will swap your elements in the array.

Also note that this function will panic if n<0\. Let’s look at the code.

# **Code:**

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    in := []int{2, 3, 5, 8}
    rand.Shuffle(len(in), func(i, j int) {
        in[i], in[j] = in[j], in[i]
    })
    fmt.Println(in)

    rand.Shuffle(len(in), func(i, j int) {
        in[i], in[j] = in[j], in[i]
    })
    fmt.Println(in)
}
```

**Output:**

It can produce a different output on your machine.

```
[5 3 2 8]
[3 5 8 2]
```

*   [array](https://golangbyexample.com/tag/array/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [shuffle](https://golangbyexample.com/tag/shuffle/)*   [slice](https://golangbyexample.com/tag/slice/)*