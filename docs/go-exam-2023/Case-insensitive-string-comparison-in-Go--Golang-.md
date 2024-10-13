<!--yml

分类：未分类

日期：2024-10-13 06:14:08

-->

# Go（Golang）中的不区分大小写字符串比较

> 来源：[`golangbyexample.com/golang-case-insensitive-string-comparison/`](https://golangbyexample.com/golang-case-insensitive-string-comparison/)

目录

+   概述

+   代码：

# **概述**

在 Golang 中，字符串采用 UTF-8 编码。GO 的**strings**包提供了一个**EqualFold**方法，可以用来进行两个字符串的不区分大小写的比较。

以下是该函数的签名。该方法返回布尔值，指示提供的两个字符串是否不区分大小写地相等。

```go
func EqualFold(s, t string) bool
```

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.EqualFold("abc", "ABC")
    fmt.Println(res)

    res = strings.EqualFold("abc", "aBC")
    fmt.Println(res)

    res = strings.EqualFold("abc", "AbC")
    fmt.Println(res)

    res = strings.EqualFold("abc", "AbCd")
    fmt.Println(res)
}
```

**输出：**

```go
true
true
true
false
```

+   [golang](https://golangbyexample.com/tag/golang/)*
