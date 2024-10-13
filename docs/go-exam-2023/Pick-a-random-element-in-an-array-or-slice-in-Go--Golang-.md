<!--yml

分类：未分类

日期：2024-10-13 06:16:09

-->

# 在 Go (Golang) 中随机选择数组或切片中的元素

> 来源：[`golangbyexample.com/pick-random-element-array-slice-go/`](https://golangbyexample.com/pick-random-element-array-slice-go/)

目录

+   概述

+   代码

# **概述**

Go 的 **‘mat/rand’** 包包含一个 **Intn** 函数，可以用来生成一个在 [0,n) 范围内的伪随机数。末尾的括号意味着 n 是排除在外的。此函数可用于在整型或字符串的数组或切片中选择一个随机元素。

要了解更多关于伪随机数的含义，请查看这篇文章 – [`golangbyexample.com/generate-random-number-golang`](https://golangbyexample.com/generate-random-number-golang)

下面是此方法的签名。它接收一个数字 n，并将返回一个范围在 0<=x<n 内的数字 x。

```go
func Intn(n int) int
```

# **代码**

我们可以直接通过索引在一个整型切片中选择元素。请参见下面的程序，从整型切片中随机选择。

```go
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

**输出：**

```go
Between 2, 5 or 6
```

+   [数组](https://golangbyexample.com/tag/array/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [选择](https://golangbyexample.com/tag/pick/)*   [切片](https://golangbyexample.com/tag/slice/)*
