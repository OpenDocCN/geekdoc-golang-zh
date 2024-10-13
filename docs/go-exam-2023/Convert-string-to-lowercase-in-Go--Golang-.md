<!--yml

分类：未分类

日期：2024-10-13 06:12:21

-->

# 在 Go (Golang) 中将字符串转换为小写

> 来源：[`golangbyexample.com/string-lowercase-golang/`](https://golangbyexample.com/string-lowercase-golang/)

目录

+   概述

+   代码：

# **概述**

在 GO 中，字符串是 UTF-8 编码的。GO 的 **strings** 包提供了一个 **ToLower** 方法，可以将所有 Unicode 字母转换为小写。该方法会返回字符串的副本，因为 GO 字符串是不可变的。

以下是该函数的签名

```go
func ToLower(s string) string
```

我们来看看实际的代码

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.ToLower("ABC")
    fmt.Println(res)

    res = strings.ToLower("ABC12$a")
    fmt.Println(res)
}
```

**输出：**

```go
abc
abc12$a
```*
