<!--yml

分类：未分类

日期：2024-10-13 06:16:30

-->

# 在 Go (Golang) 中生成一个随机字符

> 来源：[https://golangbyexample.com/generate-random-character-golang/](https://golangbyexample.com/generate-random-character-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# 概述

golang 的 **‘mat/rand’** 包包含一个 **Intn** 函数，可以用来生成一个在 [0,n) 之间的随机数。末尾的括号意味着 n 是不包含的。

要了解更多关于伪随机数的含义，请查看这篇文章 – [https://golangbyexample.com/generate-random-number-golang](https://golangbyexample.com/generate-random-number-golang)

以下是此方法的签名。它接收一个数字 n，并将返回一个范围在 0 <= x < n 的数字。

```go
func Intn(n int) int
```

上面的函数也可以用来生成一个随机字符。请参见下面的程序，它用于生成一个字符。我们还为 rand 提供了一个种子值，以便它生成不同的输出。它用于生成：

+   随机字符在小写字母 a 到 z 之间

+   随机字符在大写字母 A 和 Z 之间

+   随机字符在大写字母 A 和 Z 之间以及小写字母 a 到 z 之间

# **代码**

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    //Generate a random character between lowercase a to z
    randomChar := 'a' + rune(rand.Intn(26))
    fmt.Println(string(randomChar))

    //Generate a random character between uppercase A and Z
    randomChar = 'A' + rune(rand.Intn(26))
    fmt.Println(string(randomChar))

    //Generate a random character between uppercase A and Z  and lowercase a to z
    randomInt := rand.Intn(2)
    if randomInt == 1 {
        randomChar = 'A' + rune(rand.Intn(26))
    } else {
        randomChar = 'a' + rune(rand.Intn(26))
    }
    fmt.Println(string(randomChar))
}
```

**输出：**

```go
Will be lowercase between a to z
Will be uppercase between A to Z
Will be lowercase between a to z or uppsercase between A to Z
```

+   [字符](https://golangbyexample.com/tag/character/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [随机](https://golangbyexample.com/tag/random/)*
