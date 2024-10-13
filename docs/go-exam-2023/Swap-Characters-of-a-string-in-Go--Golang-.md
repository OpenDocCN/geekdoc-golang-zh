<!--yml

分类：未分类

日期：2024-10-13 06:13:24

-->

# 在 Go (Golang) 中交换字符串的字符

> 来源：[https://golangbyexample.com/swap-characters-string-golang/](https://golangbyexample.com/swap-characters-string-golang/)

在 Golang 中，字符串是字节的序列。字符串字面量实际上代表的是一段 UTF-8 字节序列。在 UTF-8 中，ASCII 字符对应于前 128 个 Unicode 字符，均为单字节。所有其他字符则在 1 到 4 字节之间。因此，无法直接在字符串中索引字符。在 Go 中，rune 数据类型表示一个 Unicode 点。一旦将字符串转换为 rune 数组，就可以在该数组中索引字符。你可以在这里了解更多关于 rune 的信息 – [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)。

因此，在下面的程序中，为了交换字符串中的字符，我们首先将字符串转换为 rune 数组，以便可以通过索引访问该 rune 数组中的单个字符。

```go
package main

import "fmt"
func main() {
    sample := "ab£d"
    r := []rune(sample)

    fmt.Printf("Before %s\n", string(r))
    r[2], r[3] = r[3], r[2]

    fmt.Printf("After %s\n", string(r))
}
```

**输出：**

```go
Before: ab£d
After: abd£
```
