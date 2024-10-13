<!--yml

类别：未分类

日期：2024-10-13 06:36:52

-->

# Golang正则表达式：理解插入符号和美元字符

> 来源：[https://golangbyexample.com/golang-regex-understanding-caret-and-dollar-character/](https://golangbyexample.com/golang-regex-understanding-caret-and-dollar-character/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

**插入符号字符‘^’** 和 **美元字符‘$’** 是在golang中使用的元字符。元字符是指在正则表达式中具有特殊意义的字符。下面是golang中插入符号字符和美元字符的简要介绍。

+   **插入符号字符‘^’ –** 它是一个锚字符，通常用在正则表达式的开头，以确保给定的输入字符串从开头与正则表达式匹配。它匹配输入字符串的开头

+   **美元字符‘$’ –** 它也是一个锚字符，通常用在正则表达式的末尾，以确保给定的输入字符串在末尾与正则表达式匹配。它匹配输入字符串的末尾

## **程序**

插入符号和美元元字符是分别用于匹配字符串的开始和结束的锚字符。不清楚吗？让我们通过一个例子来理解。假设你有下面的正则表达式，包含三个简单字符

```go
abc
```

这个正则表达式将匹配任何包含**abc**作为子字符串的字符串。请看下面的例子

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

上述程序会匹配所有包含**“abc”**作为子字符串的字符串。因此它会提供匹配

```go
abc
abcd
1abc23
```

基本上，它会为任何包含**“abc”**作为子字符串的输入字符串提供匹配。

如果我们只想匹配完整字符串，则需要在开头用**插入符号**字符锚定字符串，并在末尾用**美元**字符锚定。这将使我们能够进行完整字符串匹配。请看下面的例子。

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

上述程序提供匹配

```go
abc
```

但不匹配下面的内容，因为它仅匹配完整字符串

```go
abcd
1abc23
```

这是我们想匹配完整字符串的情况。如果我们还想匹配以**“abc”**开头的字符串。尾随字符无关紧要。我们该怎么做？你猜对了。在这种情况下，我们只需在正则表达式的开头使用**插入符号**字符，而不使用**美元**字符。请看下面的例子

```go
package main
import (
    "fmt"
    "regexp"
)
func main() {
    sampleRegex := regexp.MustCompile("^abc")

    match := sampleRegex.Match([]byte("abcd"))
    fmt.Printf("For abcd: %t\n", match)

    match = sampleRegex.Match([]byte("abc"))
    fmt.Printf("For abc: %t\n", match)

    match = sampleRegex.Match([]byte("1abc23"))
    fmt.Printf("For 1abc23: %t\n", match)

    match = sampleRegex.Match([]byte("ab"))
    fmt.Printf("For ab: %t\n", match)
}
```

**输出**

```go
For abcd: true
For abc: true
For 1abc23: false
For ab: false
```

它为所有以**“abc”**开头的字符串提供匹配。这就是为什么它提供匹配的原因

```go
For abcd: true
For abc: true
```

并且不匹配

```go
For 1abc23: false
For ab: false
```

如果我们想匹配字面上的**插入符号** ^，则需要用反斜杠转义它。请看下面的例子

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("\\^abc")

	match := sampleRegex.Match([]byte("^abc"))
	fmt.Printf("For ^abc: %t\n", match)

	match = sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)
}
```

**输出**

```go
For abcd: true
For abcd: false
```

如果我们还想匹配以**“abc”**结尾的字符串。起始字符无关紧要。我们该怎么做？在这种情况下，我们只需在正则表达式的开头使用**美元**字符，而不在开头使用**插入符号**字符。请看下面的例子

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("abc$")

	match := sampleRegex.Match([]byte("1abc"))
	fmt.Printf("For 1abc: %t\n", match)

	match = sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegex.Match([]byte("abcd"))
	fmt.Printf("For abcd: %t\n", match)

	match = sampleRegex.Match([]byte("ab"))
	fmt.Printf("For ab: %t\n", match)
}
```

**输出**

```go
For 1abc: true
For abc: true
For abcd: false
For ab: false
```

它为所有以**“abc”**结尾的字符串提供匹配。这就是为什么它提供匹配的原因

```go
For 1abc: true
For abc: true
```

并且不匹配

```go
For abcd: false
For ab: false
```

如果我们想匹配字面上的美元符号 $，则需要用反斜杠进行转义。请看下面的示例

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("abc\\$")

	match := sampleRegex.Match([]byte("abc$"))
	fmt.Printf("For abc$: %t\n", match)

	match = sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)
}
```

**输出**

```go
For abc$: true
For abc: false
```

此外，请查看我们的 Golang 进阶教程系列 – [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)*
