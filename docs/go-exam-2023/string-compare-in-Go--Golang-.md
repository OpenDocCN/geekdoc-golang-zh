<!--yml

类别: 未分类

日期：2024-10-13 06:11:51

-->

# Go (Golang) 中的字符串比较

> 来源：[`golangbyexample.com/compare-two-strings-golang/`](https://golangbyexample.com/compare-two-strings-golang/)

目录

**   概述

+   代码:*  *# **概述**

在 Golang 中，字符串是 UTF-8 编码的。GO 的**strings**包提供了一个**Compare**方法，可以用来比较 Go 中的两个字符串。请注意，这个方法是按字典顺序比较字符串的。

以下是该函数的签名

```go
func Compare(a, b string) int
```

正如您所注意到的，Compare 函数的返回值是一个整数。这个值将是

+   如果 a==b 则返回 0

+   如果 a < b 则返回 -1

+   如果 a > b 则返回 +1

因此，如果返回值为 0，则两个字符串相等。让我们来看一个工作程序。

# **代码:**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.Compare("abc", "abc")
    fmt.Println(res)

    res = strings.Compare("abc", "xyz")
    fmt.Println(res)

    res = strings.Compare("xyz", "abc")
    fmt.Println(res)
}
```

**输出:**

```go
0
-1
1
```

+   [比较](https://golangbyexample.com/tag/compare/)*   [两个字符串](https://golangbyexample.com/tag/two-strings/)*
