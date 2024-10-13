<!--yml

分类：未分类

日期：2024-10-13 06:14:01

-->

# 在 Go (Golang)中创建字符串的重复副本

> 来源：[`golangbyexample.com/create-count-repeating-copies-string-golang/`](https://golangbyexample.com/create-count-repeating-copies-string-golang/)

目录

+   概述**

+   代码：

# **概述**

GO 的**strings**包提供了一个**Repeat**方法，可用于创建给定字符串的重复副本。它的输入为重复次数。

以下是函数的签名。它返回字符串的副本。

```go
func Repeat(s string, count int) string
```

让我们看看有效的代码

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.Repeat("abc", 4)
    fmt.Println(res)
}
```

**输出：**

```go
abcabcabcabc
```

+   [go](https://golangbyexample.com/tag/go/)*
