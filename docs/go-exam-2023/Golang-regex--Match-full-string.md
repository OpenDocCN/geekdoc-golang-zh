<!--yml

类别：未分类

日期：2024-10-13 06:36:47

-->

# Golang正则表达式：匹配完整字符串

> 来源：[https://golangbyexample.com/golang-regex-match-full-string/](https://golangbyexample.com/golang-regex-match-full-string/)

目录

**   [概述](#Overview "Overview")**

+   [程序](#Program "Program")*  *## **概述**

Golang正则表达式包含两个锚字符，可以用来匹配完整字符串。这两个字符是

+   **插入符号‘^’ –** 这是一个锚字符，用于正则表达式的开始，以确保给定的输入字符串从开始与正则表达式匹配。它匹配输入字符串的开头。

+   **美元字符‘$’ –** 这是一个锚字符，用于正则表达式的末尾，以确保给定的输入字符串与正则表达式在末尾匹配。它匹配输入字符串的末尾。

插入符号和美元元字符是锚字符，用于分别匹配字符串的开始和结束。首先，让我们理解如果不使用这些锚字符会发生什么。假设我们有以下正则表达式，其中包含三个简单字符。

```go
abc
```

这个正则表达式将匹配任何包含**abc**作为子字符串的字符串。请看下面的示例。

```go
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegex := regexp.MustCompile("abc")
    match := sampleRegex.Match([]byte("abcd"))
    fmt.Printf("For abcd: %t\n", match)

    match = sampleRegex.Match([]byte("1abc23"))
    fmt.Printf("For 1abc23: %t\n", match)

    match = sampleRegex.Match([]byte("abc"))
    fmt.Printf("For abc: %t\n", match)
}
```

**输出**

```go
For abcd: true
For 1abc23: true
For abc: true
```

上述程序会匹配所有包含**“abc”**作为子字符串的字符串。因此，它会匹配

```go
abc
abcd
1abc23
```

基本上，它会匹配任何包含**“abc”**作为子字符串的输入字符串。

## **程序**

如果我们只想匹配完整字符串，那么我们需要在开头用**插入符号**字符和在末尾用**美元**字符锚定字符串。这将使我们能够进行完整字符串匹配。请看下面的示例。

```go
package main
import (
    "fmt"
    "regexp"
)
func main() {
    sampleRegex := regexp.MustCompile("^abc$")
    match := sampleRegex.Match([]byte("abcd"))

    fmt.Printf("For abcd: %t\n", match)
    match = sampleRegex.Match([]byte("1abc23"))
    fmt.Printf("For 1abc23: %t\n", match)

    match = sampleRegex.Match([]byte("abc"))
    fmt.Printf("For abc: %t\n", match)
}
```

**输出**

```go
For abcd: false
For 1abc23: false
For abc: true
```

在上述程序中，正则表达式是

```go
^abc$
```

上述程序会匹配

```go
abc
```

但对于下面的内容不匹配，因为它仅匹配完整字符串。

```go
abcd
1abc23
```

此外，请查看我们的Golang进阶教程系列 – [Golang进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)*
