<!--yml

类别：未分类

日期：2024-10-13 06:37:18

-->

# Golang 正则表达式：匹配原始或字面字符串

> 来源：[https://golangbyexample.com/golang-regex-literal-strin/](https://golangbyexample.com/golang-regex-literal-strin/)

目录

**[概述](#Overview "Overview")**

    +   [MatchCompile 函数](#MatchCompile_Function)

    +   [匹配方法](#Match_Method "Match Method")*  *## **概述**

在我们的示例中，我们将使用 Golang 中的 **regexp** 包，它提供了正则表达式搜索功能 [https://golang.org/pkg/regexp/](https://golang.org/pkg/regexp/)

在查看正则表达式本身之前，让我们先看一些 Go 提供的基本函数或方法来进行正则匹配。

### **MatchCompile 函数**

[https://golang.org/pkg/regexp/#MustCompile](https://golang.org/pkg/regexp/#MustCompile)

以下是函数的签名

```go
func MustCompile(str string) *Regexp
```

我们首先使用 **MustCompile** 函数编译给定的正则表达式字符串。如果给定的正则表达式无效，该函数将引发恐慌。成功编译给定的正则表达式后，它返回 regexp 结构的实例。

```go
sampleRegexp := regexp.MustCompile("some_regular_expression"")
```

### **匹配方法**

[https://golang.org/pkg/regexp/#Regexp.Match](https://golang.org/pkg/regexp/#Regexp.Match)

以下是该方法的签名

```go
func (re *Regexp) Match(b []byte) bool
```

我们可以在 regexp 实例上调用 **Match** 方法，将给定模式与正则表达式进行匹配。如果正则表达式与输入字符串匹配，则返回 true，否则返回 false。我们需要将输入字符串的字节传递给此方法。

```go
match := sampleRegexp.Match([]byte("some_string"))
```

让我们看看一个具有字面或原始字符串的基本正则表达式的简单程序

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("abc")

	match := sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegex.Match([]byte("1abc2"))
	fmt.Printf("For 1abc2: %t\n", match)

	match = sampleRegex.Match([]byte("xyz"))
	fmt.Printf("For xyz: %t\n", match)
}
```

**输出**

```go
For abc: true
For 1abc2: true
For xyz: false
```

在上面的程序中，我们有一个简单的字面字符串正则表达式

我们首先调用 **MustCompile** 来检查给定的正则表达式是否有效。之后，我们将其与下面的示例字符串或文本进行匹配

+   **字符串 “abc”** – 它匹配并返回 true

+   **字符串 “1abc2”** – 它匹配并返回 true。它匹配是因为它包含 **“abc”** 作为子字符串。

+   **字符串 “xyz”** – 它不匹配，返回 false

从上面的程序中可以看到，如果给定的字符串或文本包含正则表达式作为子字符串，它仍然会返回匹配。如果我们想进行全字符串匹配，则需要在正则表达式的开始和结束使用锚字符。**插入符号**锚字符将在开头使用，**美元符号**锚字符将在末尾使用。

有关全字符串匹配的详细信息，请参考本文

[https://golangbyexample.com/golang-regex-match-full-string](https://golangbyexample.com/golang-regex-match-full-string/)

此外，查看我们的 Golang 进阶教程系列 – [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)*
