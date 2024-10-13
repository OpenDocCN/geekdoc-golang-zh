<!--yml

类别: 未分类

日期: 2024-10-13 06:12:08

-->

# 在 Go (Golang) 中通过分隔符连接字符串

> 来源：[`golangbyexample.com/go-join-string-delimiter/`](https://golangbyexample.com/go-join-string-delimiter/)

目录

+   概述

+   代码:

# **概述**

在 GO 中，字符串是 UTF-8 编码的。**strings** 包提供了一个 **Join** 方法，可以根据分隔符连接字符串。

下面是该函数的签名

```go
func Join(a []string, sep string)
```

正如你所注意到的，这个函数接受一个字符串切片和一个分隔符，并返回一个由分隔符连接的合并字符串。分隔符或分隔符位于输入字符串切片的元素之间。请注意

+   如果输入切片的长度为零，它将返回一个空字符串

+   如果输入的分隔符为空，它将输出一个由字符串切片组合而成的字符串。

让我们看看工作代码

# **代码:**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    //Case 1 s contains sep. Will output slice of length 3
    res := strings.Join([]string{"ab", "cd", "ef"}, "-")
    fmt.Println(res)

    //Case 2 slice is empty. It will output a empty string
    res = strings.Join([]string{}, "-")
    fmt.Println(res)

    //Case 3 sep is empty. It will output a string combined from the slice of strings
    res = strings.Join([]string{"ab", "cd", "ef"}, "")
    fmt.Println(res)
}
```

**输出:**

```go
ab-cd-ef

abcdef
```*
