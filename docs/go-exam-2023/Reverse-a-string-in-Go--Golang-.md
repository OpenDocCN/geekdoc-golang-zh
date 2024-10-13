<!--yml

category: 未分类

date: 2024-10-13 06:13:32

-->

# 在Go (Golang)中反转字符串

> 来源：[https://golangbyexample.com/reverse-a-string-in-golang/](https://golangbyexample.com/reverse-a-string-in-golang/)

在Golang中，字符串是一个字节序列。字符串字面量实际上表示的是一个UTF-8字节序列。在UTF-8中，ASCII字符是单字节的，对应于前128个Unicode字符。所有其他字符的字节数在1到4字节之间。因此，无法在字符串中索引一个字符。在GO中，`rune`数据类型表示一个Unicode点。一旦字符串被转换为`rune`数组，就可以在该数组中索引字符。你可以在这里了解更多关于`rune`的内容 – [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)

因此，在下面的程序中为了反转一个字符串，我们首先将字符串转换为`rune`数组，以便可以索引该数组以获取单个字符。一旦我们获得了单个字符，就可以从末尾开始不断添加到一个新字符串中。

```go
package main

import "fmt"

func main() {
    sample := "ab£d"
    r := []rune(sample)
    var res []rune
    for i := len(r) - 1; i >= 0; i-- {
        res = append(res, r[i])
    }
    fmt.Printf("Result: %s\n", string(res))
}
```

**输出：**

```go
Result: d£ba
```
