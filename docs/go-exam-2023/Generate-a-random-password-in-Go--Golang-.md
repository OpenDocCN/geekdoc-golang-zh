<!--yml

category: 未分类

date: 2024-10-13 06:16:04

-->

# 在 Go (Golang)中生成随机密码

> 来源：[`golangbyexample.com/generate-random-password-golang/`](https://golangbyexample.com/generate-random-password-golang/)

目录

+   概述

+   代码

# **概述**

**‘math/rand’** 包含一个 Intn 函数，可用于生成[0,n)之间的伪随机数。末尾的括号表示 n 是不包含的。这个函数可以用来从字符集生成随机密码。

要了解伪随机数的含义，请查看这篇文章 – [`golangbyexample.com/generate-random-number-golang`](https://golangbyexample.com/generate-random-number-golang)

以下是该方法的签名。它接受一个数字 n 作为输入，并返回一个范围为 0<=x<n 的数字。

```go
func Intn(n int) int
```

在下面的代码中，我们首先定义了生成密码的字符集

```go
lowerCharSet   = "abcdedfghijklmnopqrst"
upperCharSet   = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
specialCharSet = "!@#$%&*"
numberSet      = "0123456789"
allCharSet     = lowerCharSet + upperCharSet + specialCharSet + numberSet
```

以下代码也处理密码必须包含特定数量的特殊字符、数字或大写字母的情况。

还请注意，一旦生成符合要求的密码，我们会在最后对其进行洗牌。为了洗牌，我们首先将其转换为一个 rune 数组。请参阅这篇文章以了解为什么需要洗牌字符串 – [`golangbyexample.com/shuffle-string-golang/`](https://golangbyexample.com/shuffle-string-golang/)

# **代码**

```go
package main

import (
    "fmt"
    "math/rand"
    "strings"
    "time"
)

var (
    lowerCharSet   = "abcdedfghijklmnopqrst"
    upperCharSet   = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    specialCharSet = "!@#$%&*"
    numberSet      = "0123456789"
    allCharSet     = lowerCharSet + upperCharSet + specialCharSet + numberSet
)

func main() {
    rand.Seed(time.Now().Unix())
    minSpecialChar := 1
    minNum := 1
    minUpperCase := 1
    passwordLength := 8
    password := generatePassword(passwordLength, minSpecialChar, minNum, minUpperCase)
    fmt.Println(password)

    minSpecialChar = 2
    minNum = 2
    minUpperCase = 2
    passwordLength = 20
    password = generatePassword(passwordLength, minSpecialChar, minNum, minUpperCase)
    fmt.Println(password)
}

func generatePassword(passwordLength, minSpecialChar, minNum, minUpperCase int) string {
    var password strings.Builder

    //Set special character
    for i := 0; i < minSpecialChar; i++ {
        random := rand.Intn(len(specialCharSet))
        password.WriteString(string(specialCharSet[random]))
    }

    //Set numeric
    for i := 0; i < minNum; i++ {
        random := rand.Intn(len(numberSet))
        password.WriteString(string(numberSet[random]))
    }

    //Set uppercase
    for i := 0; i < minUpperCase; i++ {
        random := rand.Intn(len(upperCharSet))
        password.WriteString(string(upperCharSet[random]))
    }

    remainingLength := passwordLength - minSpecialChar - minNum - minUpperCase
    for i := 0; i < remainingLength; i++ {
        random := rand.Intn(len(allCharSet))
        password.WriteString(string(allCharSet[random]))
    }
    inRune := []rune(password.String())
	rand.Shuffle(len(inRune), func(i, j int) {
		inRune[i], inRune[j] = inRune[j], inRune[i]
	})
	return string(inRune)
}
```

**输出**

输出在你的机器上会有所不同

```go
e$e&tD19
4T$&Qmg2rA4DXD5a2OGs
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [随机密码](https://golangbyexample.com/tag/random-password/)*
