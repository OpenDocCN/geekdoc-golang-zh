<!--yml

分类：未分类

日期：2024-10-13 06:37:23

-->

# Golang 正则表达式：理解点字符‘.’。

> 来源：[`golangbyexample.com/dot-chracter-golang-regex/`](https://golangbyexample.com/dot-chracter-golang-regex/)

目录

+   概述

    +   MatchCompile 函数

    +   匹配方法

+   将点作为字面字符使用

+   字符类中的点字符

## **概述**

点字符‘.’是正则表达式中最常用的元字符之一。它用于匹配任何字符。如果在正则表达式中添加特定的标志，它也可以匹配新行，稍后我们会讨论这个。默认情况下，它不匹配新行。

在查看正则表达式本身和点字符‘**.’**的用法之前，让我们看一下 Go 提供的一些基本函数或方法来进行正则匹配。

### **MatchCompile 函数**

[`golang.org/pkg/regexp/#MustCompile`](https://golang.org/pkg/regexp/#MustCompile)。以下是该函数的签名。

```go
func MustCompile(str string) *Regexp
```

我们首先使用**MustCompile**函数编译给定的正则表达式字符串。如果给定的正则表达式无效，该函数会引发错误。在成功编译给定的正则表达式后，它返回**regexp**结构的实例。

```go
sampleRegexp := regexp.MustCompile("some_regular_expression")
```

### **匹配方法**

[`golang.org/pkg/regexp/#Regexp.Match`](https://golang.org/pkg/regexp/#Regexp.Match)

以下是该方法的签名。

```go
func (re *Regexp) Match(b []byte) bool
```

我们可以在**regexp**结构实例上调用**Match**方法，将给定模式与正则表达式匹配。如果正则表达式与输入字符串匹配，则返回 true，否则返回 false。我们需要将输入字符串的字节传递给此方法。

```go
match := sampleRegexp.Match([]byte("some_string"))
```

稍后我们将在示例中看到这两个函数的实际应用。

现在让我们看一个简单的点字符‘.’程序。

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

在上述程序中，我们有一个仅包含一个点字符的简单正则表达式。

```go
sampleRegexp := regexp.MustCompile(".")
```

它匹配以下字符和字符串。

```go
a
b
ab
```

它匹配**ab**，因为默认情况下正则表达式不会匹配完整字符串，除非我们使用锚字符（插入符号和美元符号）。这就是它在‘ab’中匹配第一个字符‘a’并报告匹配的原因。

它不匹配空字符串。

让我们看另一个示例，其中正则表达式中有两个点。

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

在上述程序中，我们有一个包含两个点的简单正则表达式。

```go
sampleRegexp := regexp.MustCompile("..")
```

它将匹配包含至少两个字符的给定字符串作为子字符串。

这就是它给出匹配的原因。

```go
ab
ba
abc
```

并且不匹配

```go
a
```

如前所述，点**‘.’**也不匹配新行。但可以通过在正则表达式的开头添加一组标志来改变默认行为。我们需要添加到正则表达式开头的标志是：

```go
(?s)
```

让我们看一个相同的程序。

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

在第二个正则表达式中，我们添加了额外的标志。这就是为什么它能够匹配换行符，而第一个没有标志的正则表达式则无法匹配。

## **将点作为字面字符使用**

如果你想将**点‘.’**作为字面字符使用，我们需要用反斜杠进行转义。转义后，它将匹配一个字面点字符。例如，如果我们想匹配以下的字面字符串或文本。

```go
a.b
```

那么相应的正则表达式将是。

```go
a\.b
```

这里是相应的程序。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("a\\.b")

	match := sampleRegexp.Match([]byte("a.b"))

	fmt.Printf("For a.b string: %t\n", match)
}
```

**输出**

```go
For a.b string: true
```

## **字符类中的点字符**

点或**‘.’**在方括号或字符类内被视为字面字符。在那里面不需要转义。让我们看看一个相应的工作程序。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("[.]")
	match := sampleRegexp.Match([]byte("."))

	fmt.Println(match)

}
```

**输出**

```go
true
```

```go
Also, check out our Golang advance tutorial Series – Golang Advance Tutorial
```


