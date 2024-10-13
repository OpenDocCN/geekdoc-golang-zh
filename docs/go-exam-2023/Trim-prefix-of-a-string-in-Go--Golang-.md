<!--yml

分类：未分类

日期：2024-10-13 06:12:35

-->

# 在Go (Golang)中修剪字符串的前缀

> 来源：[https://golangbyexample.com/trim-prefix-string-go/](https://golangbyexample.com/trim-prefix-string-go/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# **概述**

在GO中，字符串是UTF-8编码的。GO的**strings**包提供了一个**TrimPrefix**方法，可用于从输入字符串中移除前缀字符串。如果输入字符串不以给定前缀开头，则输入字符串保持不变。此外，请注意此函数返回字符串的副本。

下面是该函数的签名

```go
func TrimPrefix(s, prefix string) string
```

让我们来看一下工作代码

# **代码**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {

    //This will output removed
    res := strings.TrimPrefix("testremoved", "test")
    fmt.Println(res)

    //The input string will remain unchanged as it doesn't contain the test as prefix
    res2 := strings.TrimPrefix("tesremoved", "test")
    fmt.Println(res2)
}
```

**输出**：

```go
removed
tesremoved
```*
