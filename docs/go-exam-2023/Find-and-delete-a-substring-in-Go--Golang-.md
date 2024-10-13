<!--yml

类别：未分类

日期：2024-10-13 06:13:50

-->

# 在 Go (Golang) 中查找并删除子字符串

> 来源：[`golangbyexample.com/go-find-delete-substring/`](https://golangbyexample.com/go-find-delete-substring/)

目录

**   概述

+   代码：*  *# **概述**

GO 的 **strings** 包提供了一个 **ReplaceAll** 方法，可以用来替换给定子字符串的所有不重叠实例为新的子字符串。我们可以使用这个方法通过将要删除的子字符串的替换设为空字符串来删除一个子字符串。

以下是函数的签名。该函数将在字符串 **s** 中替换所有不重叠的 **old** 实例为 **new**。要删除一个子字符串，我们可以将该子字符串作为 **old**，并将 **new** 设为空字符串。

```go
func ReplaceAll(s, old, new string) string
```

让我们来看看工作代码

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res = strings.ReplaceAll("abcabcabcdef", "ab", "")
    fmt.Println(res)
}
```

**输出：**

```go
cccdef
```*
