<!--yml

类别：未分类

日期：2024-10-13 06:13:15

-->

# 在Go（Golang）中索引字符串中的字符。

> 来源：[https://golangbyexample.com/go-index-character-string/](https://golangbyexample.com/go-index-character-string/)

在Golang中，字符串是字节的序列。字符串字面量实际上表示UTF-8字节序列。在UTF-8中，ASCII字符是单字节，对应于前128个Unicode字符。所有其他字符占用1到4个字节。因此，无法在字符串中索引字符。

例如，请看下面的程序及其输出。

```go
package main

import "fmt"

func main() {
    sample := "ab£c"
    for i := 0; i < 4; i++ {
        fmt.Printf("%c\n", sample[i])
    }
    fmt.Printf("Length is %d\n", len(sample))
}
```

**输出：**

```go
a
b
Â
£
Length is 5
```

正如你可能注意到的，它打印了与预期不同的字符，长度也是5而不是4。为什么会这样？要回答这个问题，请记住我们说过字符串本质上是字节的切片。让我们使用以下方式打印该字节切片。

```go
sample := "ab£c"
fmt.Println([]byte(sample))
```

输出将是

```go
[97 98 194 163 99]
```

这是每个字符到其字节序列的映射。如你所见，**a**、**b**、**c**各占1个字节，但**£**占用两个字节。这就是为什么字符串的长度是5而不是4。

| a | 97 |
| --- | --- |
| b | 98 |
| £ | 194, 163 |
| c | 99 |

那么我们如何在字符串中进行索引呢？这就是**rune**数据类型的作用。在Go中，**rune**数据类型表示一个Unicode点。你可以在这里了解更多关于rune的知识 - [https://golangbyexample.com/understanding-rune-in-golang](https://golangbyexample.com/understanding-rune-in-golang)

一旦字符串被转换为**rune**数组，就可以在该数组中索引字符。请看下面的代码。

```go
package main

import "fmt"

func main() {
    sample := "ab£c"
    sampleRune := []rune(sample)

    fmt.Printf("%c\n", sampleRune[0])
    fmt.Printf("%c\n", sampleRune[1])
    fmt.Printf("%c\n", sampleRune[2])
    fmt.Printf("%c\n", sampleRune[3])
}
```

**输出：**

```go
a
b
£
c
```

还需提到的是，你可以使用范围操作符迭代字符串中的所有Unicode字符，但要在字符串中索引字符，可以将其转换为**rune**数组。

+   [golang](https://golangbyexample.com/tag/golang/)*   [index](https://golangbyexample.com/tag/index/)
