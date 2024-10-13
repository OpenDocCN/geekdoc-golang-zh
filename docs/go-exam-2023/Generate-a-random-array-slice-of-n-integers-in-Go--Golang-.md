<!--yml

类别: 未分类

日期: 2024-10-13 06:16:35

-->

# 在 Go (Golang) 中生成 n 个整数的随机数组/切片

> 来源：[`golangbyexample.com/generate-random-array-slice-golang/`](https://golangbyexample.com/generate-random-array-slice-golang/)

目录

**   概述

+   代码:*  *# **概述**

GO 的 **math/rand** 包提供了一个 **Perm** 方法，可以用来生成 n 个整数的伪随机切片。数组将是范围 [0,n) 内整数的伪随机排列。

要了解更多关于伪随机数的含义，请查看这篇文章 – [`golangbyexample.com/generate-random-number-golang`](https://golangbyexample.com/generate-random-number-golang)

以下是该函数的签名。它接受一个数字 n 作为输入并返回排列后的切片。

```go
func Perm(n int) []int
```

# **代码:**

```go
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

**输出:**

```go
[6 0 1 5 9 4 2 3 7 8]
[9 8 5 0 3 4 6 7 2 1]
```

+   [数组](https://golangbyexample.com/tag/array/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [随机](https://golangbyexample.com/tag/random/)*   [切片](https://golangbyexample.com/tag/slice/)*
