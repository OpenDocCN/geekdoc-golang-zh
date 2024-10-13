<!--yml

类别：未分类

日期：2024-10-13 06:12:53

-->

# 在 Go (Golang) 中查找子字符串的第一次出现的索引

> 来源：[`golangbyexample.com/index-first-instance-substring-golang/`](https://golangbyexample.com/index-first-instance-substring-golang/)

目录

**   概述

+   代码：*  *# **概述**

在 Go 中，字符串是 UTF-8 编码的。GO 的 **strings** 包提供了一个 **Index** 方法，可以用于获取特定字符串中子字符串第一次出现的索引。如果子字符串不在给定字符串中，则返回 -1。

以下是函数的签名

```go
func Index(s, substr string) int 
```

让我们看一下工作代码

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    //Output will be 1 as "bc" is present in "abcdef" at index 1
    res := strings.Index("abcdef", "bc")
    fmt.Println(res)

    //Output will be 2 as "cd" is present in "abcdefabcdef" at index 2
    res = strings.Index("abcdefabcdef", "cd")
    fmt.Println(res)

    //Output will be -1 as "ba" is not present in "abcdef"
    res = strings.Index("abcdef", "ba")
    fmt.Println(res)
}
```

**输出：**

```go
1
2
-1
```*
