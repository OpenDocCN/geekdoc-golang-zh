<!--yml

分类：未分类

日期：2024-10-13 06:12:13

-->

# 在Go (Golang)中检查字符串是否以前缀开头

> 来源：[https://golangbyexample.com/string-begins-prefix-go/](https://golangbyexample.com/string-begins-prefix-go/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

在GO中，字符串是UTF-8编码的。GO的**strings**包提供了一个**HasPrefix**方法，可以用来检查字符串是否以某个前缀开头

以下是函数的签名

```
func HasPrefix(s, prefix string) bool
```

让我们看看工作代码

# **代码：**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    //Input string contains the prefix
    res := strings.HasPrefix("abcdef", "ab")
    fmt.Println(res)

    //Input string doesn't contain the prefix
    res = strings.HasPrefix("abcdef", "ac")
    fmt.Println(res)
}
```

**输出：**

```
true
false
```*
