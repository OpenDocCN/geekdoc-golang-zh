<!--yml

分类：未分类

日期：2024-10-13 06:13:45

-->

# 在 Go (Golang) 中查找并删除字符串中的字符

> 来源：[`golangbyexample.com/go-find-delete-character-in-string/`](https://golangbyexample.com/go-find-delete-character-in-string/)

目录

**   概述

+   代码：*  *# **概述**

GO 的 **strings** 包提供了一个 **ReplaceAll** 方法，可以用于将给定子字符串的所有不重叠实例替换为一个新的子字符串。我们可以使用此方法通过将要删除的字符的替换设为空字符串来删除一个字符。

以下是该函数的签名。该函数将在字符串 **s** 中替换所有不重叠的 **old** 实例为 **new**。要删除一个字符，我们可以将该字符作为 **old**，将 **new** 作为一个空字符串。

```go
 func ReplaceAll(s, old, new string) string
```

让我们看看工作代码。

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res = strings.ReplaceAll("abcdabxyabr", "a", "")
    fmt.Println(res)
}
```

**输出：**

```go
bcdbxybr
```

+   [golang](https://golangbyexample.com/tag/golang/)*
