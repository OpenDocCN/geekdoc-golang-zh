<!--yml

类别：未分类

日期：2024-10-13 06:36:36

-->

# 在Go（Golang）中正则表达式的串联（AND）

> 来源：[https://golangbyexample.com/concatenation-regex-golang/](https://golangbyexample.com/concatenation-regex-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

它类似于**AND**运算符。例如，如果有两个正则表达式**r1**和**r2**，它们的串联将表示如下。

```go
r1r2
```

它将匹配正则表达式**r1**后跟**r2**。**r1**和**r2**都应为有效的正则表达式。基本上，如果字符串**s**匹配正则表达式**r1**，并且字符串**t**匹配正则表达式**r2**，那么字符串**st**将匹配**r1r2**。

## **程序**

这是相应的示例程序

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	first := "abc"
	second := "xyz"
	sampleRegex := regexp.MustCompile(first + second)

	match := sampleRegex.Match([]byte("abcxyz"))
	fmt.Println(match)

	match = sampleRegex.Match([]byte("abc"))
	fmt.Println(match)

	match = sampleRegex.Match([]byte("xyz"))
	fmt.Println(match)

	match = sampleRegex.Match([]byte("abd"))
	fmt.Println(match)
}
```

**输出**

```go
true
false
false
false
```

它只匹配

```go
abcxyz
```

我们首先使用**[MustCompile](https://golang.org/pkg/regexp/#MustCompile)**函数编译给定的正则表达式。如果给定的正则表达式无效，则该函数会引发恐慌。在成功编译给定的正则表达式后，它将返回**[regexp](https://golang.org/pkg/regexp/)**结构的实例。

```go
sampleRegexp := regexp.MustCompile(first + second)
```

我们可以在正则表达式实例上调用**[Match](https://golang.org/pkg/regexp/#Match)**方法，将给定模式与正则表达式进行匹配。如果正则表达式与输入字符串匹配，则返回true，否则返回false。我们需要将输入字符串的字节传递给此方法。

```go
match := sampleRegexp.Match([]byte("abcxyz"))
```

串联也可以在三个以上的正则表达式之间进行。以下是一个示例

```go
package main

import (
    "fmt"
    "regexp"
)
func main() {
    first := "abc"
    second := "xyz"
    third := "123"
    sampleRegex := regexp.MustCompile(first + second + third)

    match := sampleRegex.Match([]byte("abcxyz123"))
    fmt.Println(match)

    match = sampleRegex.Match([]byte("abc"))
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
false
false
false
false
```

它只匹配

```go
abcxyz123
```

此外，查看我们的Golang进阶教程系列 – [Golang进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)*
