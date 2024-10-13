<!--yml

分类：未分类

日期：2024-10-13 06:12:17

-->

# 检查字符串是否以某个后缀结尾（Go 语言）

> 来源：[`golangbyexample.com/go-strings-ends-suffix/`](https://golangbyexample.com/go-strings-ends-suffix/)

目录

+   概述**

+   代码：

# **概述**

在 Go 中，字符串是 UTF-8 编码的。Go 的 **strings** 包提供了一个 **HasSuffix** 方法，可以用来检查字符串是否以某个特定后缀结尾

以下是该函数的签名

```go
func HasSuffix(s, prefix string) bool
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
    //Input string contains the suffix
    res := strings.HasSuffix("abcdef", "ef")
    fmt.Println(res)

    //Input string doesn't contain the suffix
    res = strings.HasPrefix("abcdef", "fg")
    fmt.Println(res)
}
```

**输出：**

```go
true
false
```*
