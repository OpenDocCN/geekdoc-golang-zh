<!--yml

分类：未分类

日期：2024-10-13 06:12:57

-->

# 在 Go (Golang)中将一个子字符串的所有实例替换为另一个

> 来源：[`golangbyexample.com/replace-all-instances-substring-go/`](https://golangbyexample.com/replace-all-instances-substring-go/)

目录

**   概述

+   代码：*  *# **概述**

在 GO 中，字符串是 UTF-8 编码的。GO 的**strings**包提供了一个**ReplaceAll**方法，可以用来替换给定子字符串的所有不重叠实例为一个新的子字符串。它返回字符串的副本。

下面是该函数的签名。该函数将在字符串**s**中替换所有不重叠的**old**实例为**new**。如果**old**为空，则它将在字符串**s**中的每个有效的 UTF-8 字节序列之间插入**new**。

```go
func ReplaceAll(s, old, new string) string
```

让我们来看一下工作的代码

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.ReplaceAll("abcdabxyabr", "ab", "12")
    fmt.Println(res)

    res = strings.ReplaceAll("abcdabxyabr", "", "12")
    fmt.Println(res)
}
```

**输出**

```go
12cd12xy12r
12a12b12c12d12a12b12x12y12a12b12r12
```*
