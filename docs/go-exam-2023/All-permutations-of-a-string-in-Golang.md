<!--yml

分类：未分类

日期：2024-10-13 06:13:19

-->

# Golang 中字符串的所有排列

> 来源：[`golangbyexample.com/all-permutations-string-golang/`](https://golangbyexample.com/all-permutations-string-golang/)

在 Golang 中，字符串是一系列字节。字符串字面量实际上表示的是 UTF-8 字节序列。在 UTF-8 中，ASCII 字符是单字节，对应前 128 个 Unicode 字符。所有其他字符的字节数在 1 到 4 之间。因此，不可能在字符串中对字符进行索引。在 GO 中，rune 数据类型表示一个 Unicode 点。一旦字符串被转换为 rune 数组，就可以对该数组中的字符进行索引。

你可以在这里了解更多关于符文的信息 – [`golangbyexample.com/understanding-rune-in-golang`](https://golangbyexample.com/understanding-rune-in-golang)

出于这个原因，在下面的程序中生成排列时，我们首先将字符串转换为 rune 数组，以便能够对 rune 数组进行索引，从而获取单个字符。

```go
package main

import "fmt"

func main() {
    sample := "ab£"
    sampleRune := []rune(sample)
    generatePermutation(sampleRune, 0, len(sampleRune)-1)
}

func generatePermutation(sampleRune []rune, left, right int) {
    if left == right {
        fmt.Println(string(sampleRune))
    } else {
        for i := left; i <= right; i++ {
            sampleRune[left], sampleRune[i] = sampleRune[i], sampleRune[left]
            generatePermutation(sampleRune, left+1, right)
            sampleRune[left], sampleRune[i] = sampleRune[i], sampleRune[left]
        }
    }
}
```

**输出：**

```go
ab£
a£b
ba£
b£a
£ba
£ab
```
