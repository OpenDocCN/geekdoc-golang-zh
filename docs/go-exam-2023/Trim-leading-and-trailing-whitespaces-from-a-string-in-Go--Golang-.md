<!--yml

类别：未分类

日期：2024-10-13 06:12:44

-->

# 在 Go (Golang) 中去除字符串的前导和尾随空格

> 来源：[https://golangbyexample.com/trim-whitespaces-golang/](https://golangbyexample.com/trim-whitespaces-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

在 GO 中，字符串是 UTF-8 编码的。**strings** 包提供了一个 **TrimSpace** 方法，可以用来去除前导和尾随空格。此外，请注意此函数返回的是字符串的副本。

以下是该函数的签名

```go
func TrimSpace(s string) string
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
    //This will output removed
    res := strings.TrimSpace(" test  ")
    fmt.Println(res)

    res = strings.TrimSpace(" This is test  ")
    fmt.Println(res)
}
```

**输出：**

```go
test
This is test
```*
