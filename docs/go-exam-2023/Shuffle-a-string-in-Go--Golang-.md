<!--yml

类别：未分类

日期：2024-10-13 06:16:20

-->

# 在 Go 中打乱字符串 (Golang)

> 来源：[https://golangbyexample.com/shuffle-string-golang/](https://golangbyexample.com/shuffle-string-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

**math/rand** 包提供了一个 **Shuffle** 方法，可以用来打乱字符串。该方法使用默认源对元素的顺序进行伪随机化。伪随机化意味着对于固定的输入种子，它将生成相同的随机化。因此，在我们的程序中，我们会每次用不同的种子初始化 rand 包。

以下是函数的签名。

```
func Shuffle(n int, swap func(i, j int))
```

此函数接受以下参数

+   首先是给定字符串中的总字符数。

+   第二个是一个交换函数，将为不同的索引 **i** 和 **j** 被调用。你需要提供自己的交换函数来交换字符串中的元素。

还要注意，如果 n<0，此函数将引发恐慌。在 Golang 中，字符串是字节序列。字符串字面量实际上表示一个 UTF-8 字节序列。在 UTF-8 中，ASCII 字符是单字节，对应前 128 个 Unicode 字符。所有其他字符的字节数在 1 到 4 之间。因此，无法在字符串中对字符进行索引。在 GO 中，rune 数据类型表示一个 Unicode 点。一旦字符串转换为符文数组，就可以在该数组中对字符进行索引。

你可以在这里了解更多关于上述问题的信息 – [https://golangbyexample.com/number-characters-string-golang/](https://golangbyexample.com/number-characters-string-golang/)

因此，在下面的程序中，我们首先将字符串转换为符文数组，以便我们可以通过索引访问符文数组，并使用该索引交换字符串中的字符以打乱字符串。

# **代码：**

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    in := "abcdedf£"

    inRune := []rune(in)
    rand.Shuffle(len(inRune), func(i, j int) {
        inRune[i], inRune[j] = inRune[j], inRune[i]
    })
    fmt.Println(string(inRune))

    rand.Shuffle(len(inRune), func(i, j int) {
        inRune[i], inRune[j] = inRune[j], inRune[i]
    })
    fmt.Println(string(inRune))
}
```

**输出：**

它将在你的机器上产生不同的输出。

```
dd£cebaf
feb£cadd
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [shuffle](https://golangbyexample.com/tag/shuffle/)*   [string](https://golangbyexample.com/tag/string/)*
