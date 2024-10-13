<!--yml

分类：未分类

date: 2024-10-13 06:13:10

-->

# 在 Go（Golang）中查找子字符串最后一次出现的索引

> 来源：[`golangbyexample.com/index-last-occurence-substring-go/`](https://golangbyexample.com/index-last-occurence-substring-go/)

目录

+   概述

+   代码：

# **概述**

在 GO 中，字符串是 UTF-8 编码的。GO 的 **strings** 包提供了一个 **LastIndex** 方法，可以用来获取特定字符串中子字符串最后一次出现的索引。如果给定字符串中不存在该子字符串，它将返回 -1。

以下是该函数的签名

```go
func LastIndex(s, substr string) int
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
    //Output will be 1 as "bc" is present in "abcdef" at index 1
    res := strings.LastIndex("abcdef", "bc")
    fmt.Println(res)

    //Output will be 8 as "cd" is present in "abcdefabcdef" at index 8
    res = strings.LastIndex("abcdefabcdef", "cd")
    fmt.Println(res)

    //Output will be -1 as "ba" is not present in "abcdef"
    res = strings.LastIndex("abcdef", "ba")
    fmt.Println(res)
}
```

**输出：**

```go
1
8
-1
```*
