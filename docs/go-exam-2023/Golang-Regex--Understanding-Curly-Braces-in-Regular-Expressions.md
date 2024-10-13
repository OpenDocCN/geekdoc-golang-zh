<!--yml

类别：未分类

日期：2024-10-13 06:40:57

-->

# Golang 正则表达式：理解正则表达式中的花括号

> 来源：[`golangbyexample.com/curly-braces-regex-golang/`](https://golangbyexample.com/curly-braces-regex-golang/)

目录

+   概述

+   示例

+   花括号应用于分组

+   花括号应用于字符类

+   如何在正则表达式中将花括号作为字面字符使用。

# **概述**

花括号在正则表达式中作为重复量词。它们指定前面字符在输入字符串或文本中可以出现的次数。它们也可以用于指定范围，即指定字符出现的最小和最大次数。

它的语法是

```go
{min, max}
```

其中

+   **min**表示字符可以出现的最小次数

+   **max**表示字符可以出现的最大次数

例如

```go
a{n}
```

这指定字符“a”可以恰好出现 n 次。类似地，对于以下正则表达式

```go
\d{n}
```

这指定任何数字可以恰好出现 n 次。花括号也可以用于定义范围。

例如

+   **{m,n}** – 至少**m**次，最多**n**次

+   **{m, }** – 至少**m**次

+   **{, n}** – 最多**n**次

让我们看一个相同的例子

# **示例**

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`b{2}`)

	matches := sampleRegexp.FindString("bb")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("bbb")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("bbbb")
	fmt.Println(matches)
}
```

**输出**

```go
bb
bb
bb
```

默认情况下，花括号是贪婪或非懒惰的。这意味着什么？它们会匹配所有可能的字符，并且总是更倾向于更多。也可以使花括号操作符非贪婪或懒惰。这可以通过在花括号操作符后添加问号来实现。让我们看一个相同的例子。

从输出中可以看到，在花括号操作符后添加问号操作符后，它尝试匹配尽可能少的字符，即变得非贪婪。

这就是给定正则表达式的原因

```go
ab{2,4}
```

对于以下所有输入字符串，它给出匹配**abb**

```go
abb
abbb
abbbb
```

相同的程序

```go
package main

import (
    "fmt"
    "regexp"
)

func main() {
    sampleRegexp := regexp.MustCompile(`ab{2,4}`)

    matches := sampleRegexp.FindStringSubmatch("abb")
    fmt.Println(matches)

    matches = sampleRegexp.FindStringSubmatch("abbb")
    fmt.Println(matches)

    matches = sampleRegexp.FindStringSubmatch("abbbb")
    fmt.Println(matches)
}
```

**输出**

```go
abb
abbb
abbbb
```

同时

**ab{2,4}?** 对于上述所有输入字符串，将始终给出匹配**abb**

相同的程序

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`ab{2,4}?`)

	matches := sampleRegexp.FindString("abb")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("abbb")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("abbbb")
	fmt.Println(matches)
}
```

**输出**

```go
abb
abb
abb
```

# **花括号应用于分组**

正则表达式的一部分可以放置在平衡括号内。这部分现在是一个组。我们还可以将花括号应用于此组。花括号将在分组后添加。

让我们看一个相同的例子。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile(`(ab){2}`)

	matches := sampleRegexp.FindString("abab")
	fmt.Println(matches)

	matches = sampleRegexp.FindString("ababbc")
	fmt.Println(matches)
}
```

**输出**

```go
abab
abab
```

# **花括号应用于字符类**

花括号量词也可以应用于整个字符类。其含义保持不变。字符类在正则表达式中由方括号表示。让我们看一个相同的程序。

在上述程序中，我们有以下正则表达式

```go
[ab]{4}
```

这意味着它将匹配一个长度恰好为 4 并由字符**‘a’**和**‘b’**以任何顺序组成的字符串

这就是为什么正则表达式匹配下面的字符串

```go
abab
aaaa
bbbb
aabb
bbaa
```

而且它不匹配

```go
aba - String of length 3
abbaa - String of length 5
```

# **如何在正则表达式中将大括号用作字面字符。**

转义字符可以放在开括号或闭合括号之前，如果需要以字面方式使用它们。

如果一个闭合括号前没有开括号，它会被视为字面意义上的闭合括号。

这就是在 Golang 中正则表达式中大括号的所有内容。希望你喜欢这篇文章。请在评论中分享反馈

**注意：** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尽力覆盖所有概念并提供示例。这个教程适合那些希望获得专业知识和对 Golang 有深入理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


