<!--yml

类别: 未分类

日期: 2024-10-13 06:13:06

-->

# 在 Go (Golang) 中用另一个字符替换字符串中的字符

> 来源：[https://golangbyexample.com/replace-character-string-go/](https://golangbyexample.com/replace-character-string-go/)

目录

**   [概述](#Overview "Overview")

+   [代码:](#Code "Code:")*  *# **概述**

在 GO 中，字符串是 UTF-8 编码的。GO 的 **strings** 包提供了一个 **Replace** 方法，可以用来在给定字符串中将一个字符替换为另一个字符。它返回字符串的副本。

以下是该函数的签名。

+   提供要被替换的字符作为 **old**。

+   提供你想替换的字符作为 **new**。

+   **n** 表示替换的次数。如果 **n** 为 -1，则所有 **old** 的实例将被替换为 **new**。

```go
func Replace(s, old, new string, n int)
```

让我们看看工作代码。

# **代码:**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    //It will replaces 1 instance of a with 1
    res := strings.Replace("abcdabxyabr", "a", "1", 1)
    fmt.Println(res)

    //It will replace all instances of a with 1
    res = strings.Replace("abcdabxyabr", "a", "1", -1)
    fmt.Println(res)
}
```

**输出:**

```go
1bcdabxyabr
1bcd1bxy1br
```*
