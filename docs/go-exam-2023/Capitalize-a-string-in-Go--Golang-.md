<!--yml

类别：未分类

日期：2024-10-13 06:12:30

-->

# 在 Go（Golang）中将字符串首字母大写

> 来源：[`golangbyexample.com/capitalize-string-golang/`](https://golangbyexample.com/capitalize-string-golang/)

目录

**   概述

+   代码：*  *# **概述**

在 Golang 中，字符串是 UTF-8 编码的。Go 的**strings**包提供了一个**Title**方法，可以将句子中所有单词的首字母转换为大写。它返回一个字符串的副本，所有单词的首字母都被大写。

下面是该函数的签名

```go
func Title(s string) string
```

让我们来看一下工作代码

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.Title("this is a test sentence")
    fmt.Println(res)
}
```

**输出：**

```go
This Is A Test Sentence
```*
