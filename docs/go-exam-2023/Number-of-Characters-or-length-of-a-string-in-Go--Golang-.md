<!--yml

分类：未分类

日期：2024-10-13 06:14:15

-->

# Go语言中的字符数或字符串长度

> 来源：[https://golangbyexample.com/number-characters-string-golang/](https://golangbyexample.com/number-characters-string-golang/)

在Go语言中，字符串是一系列字节。字符串字面量实际上表示的是UTF-8字节序列。在UTF-8中，ASCII字符对应于前128个Unicode字符，各占用一个字节。其他字符则占用1到4个字节。因此，使用Go内置的**len**函数无法获得字符串的确切长度。如果字符串只包含ASCII字符，可能会正常工作。但如果字符串包含非ASCII字符，则会给出正确的输出。

例如，见下面的程序及其输出。下面字符串中的**£**是一个非ASCII字符。

```
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

```
a
b
Â
£
Length is 5
```

正如你可能注意到的，它打印出不同于预期的字符，并且长度也是5而不是4。为什么会这样呢？请记住，我们说字符串本质上是字节的切片。让我们使用以下方式打印这个字节切片

```
sample := "ab£c"
fmt.Println([]byte(sample))
```

输出将是

```
[97 98 194 163 99]
```

这是每个字符与其字节序列的映射。如你所见，**a**、**b**、**c**各占1个字节，但**£**占用两个字节。这就是字符串长度为5而不是4的原因。

| a | 97 |
| --- | --- |
| b | 98 |
| £ | 194, 163 |
| c | 99 |

有两种方法可以获取字符串的正确长度

+   使用rune数据类型。**rune**数据类型表示一个Unicode点。一旦字符串转换为**rune**数组，就可以获得字符串的正确长度。

+   使用range操作符遍历字符串并计算长度。range操作符遍历字符串中的UTF-8字符。

以下代码说明了上述两点。

```
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

```
4
4
```
