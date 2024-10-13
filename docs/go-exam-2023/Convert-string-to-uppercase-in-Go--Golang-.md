<!--yml

类别：未分类

日期：2024-10-13 06:12:26

-->

# 在 Go（Golang）中将字符串转换为大写

> 来源：[`golangbyexample.com/golang-string-uppercase/`](https://golangbyexample.com/golang-string-uppercase/)

目录

+   概述

+   代码：

# **概述**

在 Go 语言中，字符串采用 UTF-8 编码。**strings** 包提供了一个 **ToUpper** 方法，可以将所有 Unicode 字母转换为大写。此方法将返回字符串的副本，因为 Go 中的字符串是不可变的。

以下是该函数的签名

让我们看看实际代码

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.ToUpper("abc")
    fmt.Println(res)

    res = strings.ToUpper("abc12$")
    fmt.Println(res)
}
```

**输出：**

[预览 1]*
