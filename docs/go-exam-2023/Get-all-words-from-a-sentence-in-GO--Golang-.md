<!--yml

类别：未分类

日期：2024-10-13 06:12:04

-->

# 从 GO（Golang）中的句子获取所有单词

> 来源：[`golangbyexample.com/words-from-sentence-golang/`](https://golangbyexample.com/words-from-sentence-golang/)

目录

**   概述

+   代码:*  *# **概述**

在 GO 中，字符串是 UTF-8 编码的。GO 的 **strings** 包提供了一个 **Fields** 方法，可以用于根据一个或多个连续空白字符的实例来分割字符串。

以下是函数的签名

```go
func Fields(s string) []string 
```

正如你所注意到的，它返回了输入字符串的子字符串切片。此外，请注意，如果输入字符串为空，它将返回一个空切片。

+   输入字符串为空

+   输入字符串仅包含空白字符

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    //Case 1 Input string contains single spaces
    res := strings.Fields("ab cd ef")
    fmt.Println(res)

    //Case 2 Input string doesn't contain white spaces
    res = strings.Fields("abcdef")
    fmt.Println(res)

    //Case 3 Input string contains double white spaces and spaces at end too.
    res = strings.Fields("ab  cd   ef ")
    fmt.Println(res)

    //Case 4: Input string contain white spaces only. Will output a empty slice
    res = strings.Fields("  ")
    fmt.Println(res)

    //Case 5: Input string is empty. Will output a empty slice
    res = strings.Fields("")
    fmt.Println(res)
}
```

**输出：**

```go
[ab cd ef]
[abcdef]
[ab cd ef]
[]
[]
```*
