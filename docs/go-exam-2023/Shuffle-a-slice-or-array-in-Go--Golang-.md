<!--yml

分类：未分类

日期：2024-10-13 06:16:25

-->

# 在 Go (Golang) 中洗牌一个切片或数组

> 来源：[https://golangbyexample.com/shuffle-slice-or-array-go/](https://golangbyexample.com/shuffle-slice-or-array-go/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code：")*  *# **概述**

Go 的 **math/rand** 包提供了一个 **Shuffle** 方法，可以用于洗牌数组或切片。该方法使用默认源伪随机化元素的顺序。伪随机化意味着对于固定的输入种子，它将生成相同的随机化。这就是为什么在我们的程序中，每次我们会使用不同的种子初始化 rand 包。

以下是函数的签名。

```go
func Shuffle(n int, swap func(i, j int))
```

该函数接受参数

+   第一个是数组或切片的长度。

+   第二个是一个交换函数，它会被调用用于不同的索引 **i** 和 **j**。你需要提供自己的交换函数，用于交换数组中的元素。

另外请注意，如果 n<0，这个函数会引发恐慌。让我们来看一下代码。

# **代码：**

```go
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

**输出：**

它可能会在你的机器上产生不同的输出。

```go
[5 3 2 8]
[3 5 8 2]
```

+   [数组](https://golangbyexample.com/tag/array/)*   [Go](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*   [洗牌](https://golangbyexample.com/tag/shuffle/)*   [切片](https://golangbyexample.com/tag/slice/)*
