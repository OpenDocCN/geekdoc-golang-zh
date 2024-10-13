<!--yml  

分类：未分类  

日期：2024-10-13 06:35:17  

-->  

# Golang正则表达式 – 在方括号或字符类中包含点 – ‘.’  

> 来源：[https://golangbyexample.com/dot-square-bracket-regex-golang/](https://golangbyexample.com/dot-square-bracket-regex-golang/)  

目录  

**   [概述](#Overview "Overview")  

+   [程序](#Program "Program")*  *## **概述**  

点或**‘.’**在方括号或字符类内被视为字面字符。在那里面不需要转义。让我们看看一个工作的程序。  

我们将在示例中使用**regexp**包，它提供正则表达式搜索功能。  

[https://golang.org/pkg/regexp/](https://golang.org/pkg/regexp/)  

## **程序**

```
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

```
true
```

此正则表达式  

```
[.]
```

将匹配单个点或**‘.’**  

我们首先使用MustCompile函数编译给定的正则表达式。如果给定的正则表达式无效，此函数会导致恐慌。成功编译后，它返回regexp结构的实例。  

```
sampleRegexp := regexp.MustCompile("[.]")
```

我们可以在regexp实例上调用Match方法，将给定模式与正则表达式进行匹配。如果正则表达式与输入字符串匹配，则返回true，否则返回false。我们需要将输入字符串的字节传递给此方法。  

```
match := sampleRegexp.Match([]byte("."))
```

此外，请查看我们的Golang高级教程系列 – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)  

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*  
