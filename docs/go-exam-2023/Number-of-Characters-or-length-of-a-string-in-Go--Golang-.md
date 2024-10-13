<!--yml

分类：未分类

日期：2024-10-13 06:14:15

-->

# Go 语言中的字符数或字符串长度

> 来源：[`golangbyexample.com/number-characters-string-golang/`](https://golangbyexample.com/number-characters-string-golang/)

在 Go 语言中，字符串是一系列字节。字符串字面量实际上表示的是 UTF-8 字节序列。在 UTF-8 中，ASCII 字符对应于前 128 个 Unicode 字符，各占用一个字节。其他字符则占用 1 到 4 个字节。因此，使用 Go 内置的**len**函数无法获得字符串的确切长度。如果字符串只包含 ASCII 字符，可能会正常工作。但如果字符串包含非 ASCII 字符，则会给出正确的输出。

例如，见下面的程序及其输出。下面字符串中的**£**是一个非 ASCII 字符。

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

正如你可能注意到的，它打印出不同于预期的字符，并且长度也是 5 而不是 4。为什么会这样呢？请记住，我们说字符串本质上是字节的切片。让我们使用以下方式打印这个字节切片

```go
sample := "ab£c"
fmt.Println([]byte(sample))
```

输出将是

```go
[97 98 194 163 99]
```

这是每个字符与其字节序列的映射。如你所见，**a**、**b**、**c**各占 1 个字节，但**£**占用两个字节。这就是字符串长度为 5 而不是 4 的原因。

| a | 97 |
| --- | --- |
| b | 98 |
| £ | 194, 163 |
| c | 99 |

有两种方法可以获取字符串的正确长度

+   使用 rune 数据类型。**rune**数据类型表示一个 Unicode 点。一旦字符串转换为**rune**数组，就可以获得字符串的正确长度。

+   使用 range 操作符遍历字符串并计算长度。range 操作符遍历字符串中的 UTF-8 字符。

以下代码说明了上述两点。

```go
package main

import "fmt"

func main() {
    sample := "ab£c"
    //Using rune
    s := []rune(sample)
    fmt.Println(len(s))

    //Using range
    len := 0
    for range sample {
        len++
    }
    fmt.Println(len)
}
```

**输出：**

```go
4
4
```
