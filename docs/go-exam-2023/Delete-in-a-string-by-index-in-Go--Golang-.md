<!--yml

类别：未分类

日期：2024-10-13 06:13:55

-->

# 在 Go (Golang)中按索引删除字符串

> 来源：[`golangbyexample.com/golang-delete-index-string/`](https://golangbyexample.com/golang-delete-index-string/)

在 Golang 中，字符串是字节的序列。字符串字面量实际上表示的是 UTF-8 字节序列。在 UTF-8 中，ASCII 字符是对应前 128 个 Unicode 字符的单字节。所有其他字符的字节数在 1 到 4 之间。因此，无法对字符串中的字符进行索引。在 GO 中，rune 数据类型表示一个 Unicode 点。一旦字符串被转换为 rune 数组，就可以对该 rune 数组中的字符进行索引。

你可以在这里了解更多关于 rune 的信息 – [`golangbyexample.com/understanding-rune-in-golang`](https://golangbyexample.com/understanding-rune-in-golang)

因此，在下面的程序中，我们通过索引删除给定字符串中的字符，首先将字符串转换为 rune 数组，以便可以对 rune 数组进行索引，然后通过索引删除字符。

```go
package main

import "fmt"

func main() {
    sample := "ab£c"
    s := []rune(sample)
    res := delChar(s, 2)
    fmt.Println(string(res))
}

func delChar(s []rune, index int) []rune {
    return append(s[0:index], s[index+1:]...)
}
```

**输出：**

```go
abc
```
