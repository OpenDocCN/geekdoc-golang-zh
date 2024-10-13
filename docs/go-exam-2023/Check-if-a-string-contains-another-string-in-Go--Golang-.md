<!--yml

category: 未分类

date: 2024-10-13 06:11:55

-->

# 检查字符串在Go (Golang)中是否包含另一个字符串

> 来源：[https://golangbyexample.com/check-if-substring-golang/](https://golangbyexample.com/check-if-substring-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

在Golang中，字符串是UTF-8编码的。GO的**strings**包提供了一个**Contains**方法，可以用来检查特定字符串是否是另一个字符串的子字符串。

以下是函数的签名

```
func Contains(s, substr string) bool
```

如你所见，Compare函数的返回值是一个布尔值。这个值将是

+   true是**substr**在**s**中存在

+   false是**substr**在**s**中不存在

# **代码：**

```
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

```
true
false
```*
