<!--yml  

分类：未分类  

日期：2024-10-13 06:35:17  

-->  

# Golang 正则表达式 – 在方括号或字符类中包含点 – ‘.’  

> 来源：[`golangbyexample.com/dot-square-bracket-regex-golang/`](https://golangbyexample.com/dot-square-bracket-regex-golang/)  

目录  

**   概述  

+   程序

## **概述**  

点或**‘.’**在方括号或字符类内被视为字面字符。在那里面不需要转义。让我们看看一个工作的程序。  

我们将在示例中使用**regexp**包，它提供正则表达式搜索功能。  

[`golang.org/pkg/regexp/`](https://golang.org/pkg/regexp/)  

## **程序**

```go
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegex := regexp.MustCompile("[.]")
    match := sampleRegex.Match([]byte("."))
    fmt.Println(match)
}
```

**输出**  

```go
true
```

此正则表达式  

```go
[.]
```

将匹配单个点或**‘.’**  

我们首先使用 MustCompile 函数编译给定的正则表达式。如果给定的正则表达式无效，此函数会导致恐慌。成功编译后，它返回 regexp 结构的实例。  

```go
sampleRegexp := regexp.MustCompile("[.]")
```

我们可以在 regexp 实例上调用 Match 方法，将给定模式与正则表达式进行匹配。如果正则表达式与输入字符串匹配，则返回 true，否则返回 false。我们需要将输入字符串的字节传递给此方法。  

```go
match := sampleRegexp.Match([]byte("."))
```

此外，请查看我们的 Golang 高级教程系列 – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)  

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*  
