<!--yml

分类：未分类

日期：2024-10-13 06:10:04

-->

# 在 Go (Golang) 中删除或去掉字符串中的所有空白字符。

> 来源：[`golangbyexample.com/remove-all-white-spaces-string-golang/`](https://golangbyexample.com/remove-all-white-spaces-string-golang/)

**strings.ReplaceAll** 函数可以用来去掉 Golang 字符串中的所有空白字符。以下是该函数的签名：

```go
func ReplaceAll(s, old, new string) string
```

+   **s**（第一个参数）是输入字符串。

+   **old**（第二个参数）是要被替换的字符串，**new**（第三个参数）是替换后的字符串。

**有效代码：**

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    sample := " This is a sample string   "
    noSpaceString := strings.ReplaceAll(sample, " ", "")
    fmt.Println(noSpaceString)
}
```

**输出：**

```go
Thisisasamplestring
```

+   [所有](https://golangbyexample.com/tag/all/)*   [修剪](https://golangbyexample.com/tag/trim/)
