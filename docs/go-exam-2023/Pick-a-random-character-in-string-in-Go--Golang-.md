<!--yml

类别：未分类

日期：2024-10-13 06:16:15

-->

# 在 Go (Golang) 中随机选择字符串中的字符

> 来源：[https://golangbyexample.com/pick-random-character-string-golang/](https://golangbyexample.com/pick-random-character-string-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# **概述**

**‘mat/rand’** 包含一个 **Intn** 函数，可以用来生成一个在 [0,n) 之间的伪随机数。末尾的括号意味着 n 是不包括的。这个函数可以用来从字符串中选择一个随机元素。我们可以在 0 到字符串长度减 1 之间生成一个随机数。然后我们可以用这个随机数来索引字符串并得到结果。

但是上述方法存在一个问题。在 Golang 中，字符串是字节的序列。字符串字面量实际上表示的是 UTF-8 字节序列。在 UTF-8 中，ASCII 字符是对应于前 128 个 Unicode 字符的单字节。所有其他字符占用 1 到 4 个字节。因此，无法在字符串中索引一个字符。在 Go 中，rune 数据类型表示一个 Unicode 点。一旦字符串被转换为 rune 数组，就可以在该数组中索引字符。

你可以在这里了解更多关于上述问题的信息 – [https://golangbyexample.com/number-characters-string-golang/](https://golangbyexample.com/number-characters-string-golang/)

因此，在下面的程序中，为了从给定字符串中选择一个随机字符，我们首先将字符串转换为 rune 数组，以便能够索引这个 rune 数组，然后返回随机字符。

# **代码**

```go
package main

import (
    "fmt"
    "math/rand"
)

func main() {
    in := "abcdedf£"
    inRune := []rune(in)
    randomIndex := rand.Intn(len(inRune))
    pick := inRune[randomIndex]
    fmt.Println(string(pick))
}
```

**输出：**

```go
One of a,b,c,d,e,f,£
```

+   [字符](https://golangbyexample.com/tag/character/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [选择](https://golangbyexample.com/tag/pick/)*   [随机](https://golangbyexample.com/tag/random/)*   [字符串](https://golangbyexample.com/tag/string/)*
