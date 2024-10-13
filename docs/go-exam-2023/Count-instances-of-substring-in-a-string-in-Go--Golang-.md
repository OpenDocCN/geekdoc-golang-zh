<!--yml

分类：未分类

日期：2024-10-13 06:12:48

-->

# 在Go（Golang）中计算字符串中子字符串的实例

> 来源：[https://golangbyexample.com/instances-substring-string-go/](https://golangbyexample.com/instances-substring-string-go/)

目录

**   [概述](#Overview "概述")**

+   [代码：](#Code "Code:")*  *# **概述**

在GO中，字符串采用UTF-8编码。GO的**strings**包提供了一个**Count**方法，可以用来获取特定字符串中非重叠子字符串的数量。

以下是该函数的签名

```go
func Count(s, substr string) int
```

另外请注意，如果传递给函数的**substr**是一个空字符串，则返回值将为1加上给定字符串中的Unicode字符点数。

让我们看一下工作代码

# **代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {

    //Case 1 Input string contains the substring
    res := strings.Count("abcdabcd", "ab")
    fmt.Println(res)

    //Case 1 Input string doesn't contains the substring
    res = strings.Count("abcdabcd", "xy")
    fmt.Println(res)

    //Case 1 Substring supplied is an empty string
    res = strings.Count("abcdabcd", "")
    fmt.Println(res)
}
```

**输出：**

```go
2
0
9
```*
