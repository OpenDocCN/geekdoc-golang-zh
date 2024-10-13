<!--yml

类别：未分类

日期：2024-10-13 06:11:59

-->

# 在 Go(Golang)中拆分字符串

> 来源：[`golangbyexample.com/split-a-string-golang/`](https://golangbyexample.com/split-a-string-golang/)

目录

+   概述

+   代码

# **概述**

在 Golang 中，字符串是 UTF-8 编码的。GO 的**strings**包提供了一个可以根据分隔符拆分字符串的**Split**方法。

以下是该函数的签名：

```go
func Split(s, sep string) []string
```

正如你所注意到的，**Split**函数的返回值是一个字符串切片。让我们注意一下这个方法的几点。

+   案例 1：**Split**方法将**s**拆分成由**sep**分隔的子字符串。它将返回一个字符串切片。

+   案例 2：如果**s**不包含**sep**，则返回一个长度为 1 的字符串切片。这个切片的唯一元素将是**s**。

+   案例 3：如果**sep**为空，则会在将**s**拆分为每个 UTF-8 序列后返回一个字符串切片。

+   案例 4：如果**s**和**sep**都为空，它将返回一个空的字符串切片。

# **代码**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    //Case 1 s contains sep. Will output slice of length 3
    res := strings.Split("ab$cd$ef", "$")
    fmt.Println(res)

    //Case 2 s doesn't contain sep. Will output slice of length 1
    res = strings.Split("ab$cd$ef", "-")
    fmt.Println(res)

    //Case 3 sep is empty. Will output slice of length 8
    res = strings.Split("ab$cd$ef", "")
    fmt.Println(res)

    //Case 4 both s and sep are empty. Will output empty slice
    res = strings.Split("", "")
    fmt.Println(res)
}
```

**输出：**

```go
[ab cd ef]
[ab$cd$ef]
[a b $ c d $ e f]
[]
```*
