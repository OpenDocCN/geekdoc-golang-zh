<!--yml

category: 未分类

date: 2024-10-13 06:11:55

-->

# 检查字符串在 Go (Golang)中是否包含另一个字符串

> 来源：[`golangbyexample.com/check-if-substring-golang/`](https://golangbyexample.com/check-if-substring-golang/)

目录

+   概述

+   代码：

# **概述**

在 Golang 中，字符串是 UTF-8 编码的。GO 的**strings**包提供了一个**Contains**方法，可以用来检查特定字符串是否是另一个字符串的子字符串。

以下是函数的签名

```go
func Contains(s, substr string) bool
```

如你所见，Compare 函数的返回值是一个布尔值。这个值将是

+   true 是**substr**在**s**中存在

+   false 是**substr**在**s**中不存在

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    present := strings.Contains("abc", "ab")
    fmt.Println(present)

    present = strings.Contains("abc", "xyz")
    fmt.Println(present)
}
```

**输出：**

```go
true
false
```*
