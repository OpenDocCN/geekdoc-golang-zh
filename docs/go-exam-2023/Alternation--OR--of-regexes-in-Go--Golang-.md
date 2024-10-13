<!--yml

类别：未分类

日期：2024-10-13 06:36:41

-->

# Go（Golang）中的正则表达式的交替（OR）

> 来源：[`golangbyexample.com/alternation-regex-golang/`](https://golangbyexample.com/alternation-regex-golang/)

目录

**   概述

+   程序*  *## **概述**

它类似于 OR 操作。通过使用|运算符连接两个正则表达式。如果有两个正则表达式**r1**和**r2**，则交替表示如下

```go
r1|r2
```

它将匹配**r1**或**r2**，优先考虑**r1**。基本上，如果字符串**s**匹配正则表达式**r1**，字符串**t**匹配正则表达式**r2**，那么**r1|r2**将匹配**s**或**t**。当我们说将优先考虑**r1**时，这意味着如果在给定的样本字符串中，首先会尝试匹配**r1**，如果找不到**r1**，则会尝试匹配**r2**。

## **程序**

```go
package main
import (
    "fmt"
    "regexp"
)
func main() {
    sampleRegex := regexp.MustCompile("abc|xyz")
    match := sampleRegex.Match([]byte("abc"))

    fmt.Println(match)
    match = sampleRegex.Match([]byte("xyz"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abcxyz"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abd"))
    fmt.Println(match)
}
```

**输出**

```go
true
true
true
false
```

它匹配

```go
abc
xyz
```

它也匹配。

```go
abcxyz
```

这是因为它匹配前缀**“abc”**并给出了真实匹配。

此外，它不匹配

```go
abd
```

交替也可以在超过两个正则表达式之间进行。下面是一个示例

```go
package main
import (
    "fmt"
    "regexp"
)
func main() {
    sampleRegex := regexp.MustCompile("abc|xyz|123")

    match := sampleRegex.Match([]byte("abc"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("xyz"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("123"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abcxyz123"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abd"))
    fmt.Println(match)
}
```

**输出**

```go
true
true
true
true
false
```

它匹配

```go
abc
xyz
123
```

它也匹配

```go
abcxyz123
```

这是因为它匹配前缀**“abc”**并给出了真实匹配。

此外，它不匹配

```go
abd
```

此外，请查看我们的 Golang 高级教程系列——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
