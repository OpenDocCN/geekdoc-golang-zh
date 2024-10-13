<!--yml

类别：未分类

日期：2024-10-13 06:41:02

-->

# Golang 正则匹配任何字符

> 来源：[`golangbyexample.com/golang-regex-match-any-character/`](https://golangbyexample.com/golang-regex-match-any-character/)

目录

**   概述

+   程序*  *# **概述**

点 ‘.’ 字符是正则表达式中最常用的元字符之一。它用于匹配任何字符。默认情况下，它不匹配新行。

# **程序**

现在让我们看看一个简单的点 ‘.’ 字符程序

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(".")

	match := sampleRegexp.Match([]byte("a"))
	fmt.Printf("For a: %t\n", match)

	match = sampleRegexp.Match([]byte("b"))
	fmt.Printf("For b: %t\n", match)

	match = sampleRegexp.Match([]byte("ab"))
	fmt.Printf("For ab: %t\n", match)

	match = sampleRegexp.Match([]byte(""))
	fmt.Printf("For empty string: %t\n", match)
}
```

**输出**

```go
For a: true
For b: true
For ab: true
For empty string: false
```

在上述程序中，我们有一个简单的正则表达式，仅包含一个点字符。

```go
sampleRegexp := regexp.MustCompile(".")
```

它匹配下面的字符和字符串。

```go
a
b
ab
```

它匹配 **ab**，因为默认情况下正则表达式不会匹配完整字符串，除非我们使用锚字符（插入符号和美元字符）。这就是它匹配字符串 ‘ab’ 中第一个字符 ‘a’ 并报告匹配的原因。

它不匹配空字符串。

让我们看看另一个例子，其中正则表达式中有两个点。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("..")
	match := sampleRegexp.Match([]byte("ab"))
	fmt.Printf("For ab: %t\n", match)

	match = sampleRegexp.Match([]byte("ba"))
	fmt.Printf("For ba: %t\n", match)

	match = sampleRegexp.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegexp.Match([]byte("a"))
	fmt.Printf("For a: %t\n", match)
}
```

**输出**

```go
For ab: true
For ba: true
For abc: true
For a: false
```

在上述程序中，我们有一个简单的正则表达式，包含两个点。

```go
sampleRegexp := regexp.MustCompile("..")
```

它将匹配任何包含至少两个字符作为子字符串的给定字符串。

这就是为什么它给出匹配的原因

```go
ab
ba
abc
```

并且不匹配

```go
a
```

点 **‘.’** 如前所述，不匹配新行。但默认行为可以通过在正则表达式开头添加一组标志来更改。我们需要添加到正则表达式开头的标志是：

```go
(?s)
```

让我们看看相同的程序

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(".")

	match := sampleRegexp.Match([]byte("\n"))
	fmt.Printf("For \\n: %t\n", match)

	sampleRegexp = regexp.MustCompile("(?s).")

	match = sampleRegexp.Match([]byte("\n"))
	fmt.Printf("For \\n: %t\n", match)
}
```

**输出**

```go
For \n: false
For \n: true
```

```go
sampleRegexp := regexp.MustCompile(".")
```

和

```go
sampleRegexp = regexp.MustCompile("(?s).")
```

在第二个正则表达式中，我们添加了额外的标志。这就是为什么它在新行上给出匹配，而第一个没有标志的正则表达式不匹配的原因。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
