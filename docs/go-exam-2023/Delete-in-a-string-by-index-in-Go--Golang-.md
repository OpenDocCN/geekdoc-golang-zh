<!--yml

类别：未分类

日期：2024-10-13 06:13:55

-->

# 在Go (Golang)中按索引删除字符串

> 来源：[https://golangbyexample.com/golang-delete-index-string/](https://golangbyexample.com/golang-delete-index-string/)

在Golang中，字符串是字节的序列。字符串字面量实际上表示的是UTF-8字节序列。在UTF-8中，ASCII字符是对应前128个Unicode字符的单字节。所有其他字符的字节数在1到4之间。因此，无法对字符串中的字符进行索引。在GO中，rune数据类型表示一个Unicode点。一旦字符串被转换为rune数组，就可以对该rune数组中的字符进行索引。

你可以在这里了解更多关于rune的信息 – [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)

因此，在下面的程序中，我们通过索引删除给定字符串中的字符，首先将字符串转换为rune数组，以便可以对rune数组进行索引，然后通过索引删除字符。

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
