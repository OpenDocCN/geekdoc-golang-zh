<!--yml

分类：未分类

日期：2024-10-13 06:16:46

-->

# 在 Go（Golang）中生成随机字符串

> 来源：[`golangbyexample.com/generate-random-string-golang/`](https://golangbyexample.com/generate-random-string-golang/)

目录

**   概述

+   代码*  *# **概述**

**‘mat/rand’**包的 golang 包含一个**Intn**函数，可用于生成一个介于[0,n)之间的伪随机数。末尾的括号表示 n 是排除的。该函数可以用来从字符集生成一个随机字符串。

要了解更多关于伪随机数的含义，请查看这篇文章 – [`golangbyexample.com/generate-random-number-golang`](https://golangbyexample.com/generate-random-number-golang)

下面是该方法的签名。它输入一个数字 n，并返回一个范围在 0<=x<n 内的数字 x。

```go
func Intn(n int) int
```

上述函数可用于生成随机字符串。基本上，我们首先选择一个 charSet。然后我们使用上述函数生成一个随机数，再用这个随机数从 charSet 中获取一个随机字符。这个随机字符被添加到字符串中，直到我们得到一个所需长度的随机字符串。

# **代码**

```go
package main

import (
    "fmt"
    "math/rand"
    "strings"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    //Only lowercase
    charSet := "abcdedfghijklmnopqrst"
    var output strings.Builder
    length := 10
    for i := 0; i < length; i++ {
        random := rand.Intn(len(charSet))
        randomChar := charSet[random]
        output.WriteString(string(randomChar))
    }
    fmt.Println(output.String())
    output.Reset()

    //Lowercase and Uppercase Both
    charSet = "abcdedfghijklmnopqrstABCDEFGHIJKLMNOP"
    length = 20
    for i := 0; i < length; i++ {
        random := rand.Intn(len(charSet))
        randomChar := charSet[random]
        output.WriteString(string(randomChar))
    }
    fmt.Println(output.String())
}
```

**输出：**

下面是我机器上的输出。在你的机器上可能会给出不同的输出。

```go
himsemkpkd
nHaiEpccEdBfCFPtaBbi
```

在上面的程序中，我们使用的字符集为

```go
abcdedfghijklmnopqrst and abcdedfghijklmnopqrstABCDEFGHIJKLMNOP
```

在上述字符集中，所有字符都是 ASCII 字符，因此我们能够在**charSet**字符串中索引一个字符。但如果**charSet**包含非 ASCII 字符，这可能会成为一个问题。

在 Golang 中，字符串是字节的序列。字符串字面量实际上表示 UTF-8 字节序列。在 UTF-8 中，ASCII 字符是单字节的，对应前 128 个 Unicode 字符。所有其他字符占用 1 到 4 个字节。因此，不可能在字符串中索引一个字符。在 GO 中，rune 数据类型表示一个 Unicode 点。一旦字符串被转换为一个 rune 数组，就可以在该数组中索引字符。

因此，如果字符集包含一些非 ASCII 字符，它们可能会占用多个字节。在这种情况下，我们不能使用上述代码生成随机字符串，因为无法在 charSet 中索引。对此，我们必须首先将字符串转换为 rune 数组，以便能够在 rune 数组中索引字符，然后逐步形成随机字符串。

在下面的示例中，我们的 charSet 包含一个非 ASCII 字符**'£'**。该字符占用两个字节。

```go
package main

import (
    "fmt"
    "math/rand"
    "strings"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())
    //Only lowercase and £
    charSet := []rune("abcdedfghijklmnopqrst£")
    var output strings.Builder
    length := 10
    for i := 0; i < length; i++ {
        random := rand.Intn(len(charSet))
        randomChar := charSet[random]
        output.WriteRune(randomChar)
    }
    fmt.Println(output.String())
    output.Reset()

   //Lowercase and Uppercase Both and £
    charSet = []rune("abcdedfghijklmnopqrstABCDEFGHIJKLMNOP£")
    length = 20
    for i := 0; i < length; i++ {
        random := rand.Intn(len(charSet))
        randomChar := charSet[random]
        output.WriteRune(randomChar)
    }
    fmt.Println(output.String())
}
```

**输出**：

下面是我机器上的输出。在你的机器上可能会给出不同的输出。

```go
aidqpbse£j
rebhjblsePsLpGBPOhfB
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [random](https://golangbyexample.com/tag/random/)*   [string](https://golangbyexample.com/tag/string/)*
