<!--yml

类别：未分类

日期：2024-10-13 06:12:40

-->

# 在Go语言中修剪字符串的后缀

> 来源：[https://golangbyexample.com/trim-suffix-string-golang/](https://golangbyexample.com/trim-suffix-string-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

在GO中，字符串是UTF-8编码的。GO的**strings**包提供了一个**TrimSuffix**方法，可以用来从输入字符串中移除后缀字符串。如果输入字符串不以给定的后缀结尾，那么输入字符串将保持不变。同时，请注意该函数返回的是字符串的副本。

以下是该函数的签名

```
func TrimSuffix(s, suffix string)
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
    //This will output "removed"
    res := strings.TrimSuffix("removedtest", "test")
    fmt.Println(res)

    //The input string will remain unchanged as it doesn't contain the test as suffix
    res2 := strings.TrimSuffix("removedtes", "test")
    fmt.Println(res2)
}
```

**输出：**

```
removed
removedtes
```*
