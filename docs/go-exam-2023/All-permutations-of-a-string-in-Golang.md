<!--yml

分类：未分类

日期：2024-10-13 06:13:19

-->

# Golang中字符串的所有排列

> 来源：[https://golangbyexample.com/all-permutations-string-golang/](https://golangbyexample.com/all-permutations-string-golang/)

在Golang中，字符串是一系列字节。字符串字面量实际上表示的是UTF-8字节序列。在UTF-8中，ASCII字符是单字节，对应前128个Unicode字符。所有其他字符的字节数在1到4之间。因此，不可能在字符串中对字符进行索引。在GO中，rune数据类型表示一个Unicode点。一旦字符串被转换为rune数组，就可以对该数组中的字符进行索引。

你可以在这里了解更多关于符文的信息 – [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)

出于这个原因，在下面的程序中生成排列时，我们首先将字符串转换为rune数组，以便能够对rune数组进行索引，从而获取单个字符。

```
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

```
ab£
a£b
ba£
b£a
£ba
£ab
```
