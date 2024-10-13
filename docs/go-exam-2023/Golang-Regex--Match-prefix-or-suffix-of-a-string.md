<!--yml

类别：未分类

日期：2024-10-13 06:36:57

-->

# Golang 正则表达式：匹配字符串的前缀或后缀

> 来源：[https://golangbyexample.com/regex-prefix-suffix-golang/](https://golangbyexample.com/regex-prefix-suffix-golang/)

目录

**   [概述](#Overview "概述")

+   [前缀匹配](#Prefix_Matching "前缀匹配")

+   [后缀匹配](#Suffix_Matching "后缀匹配")*  *## **概述**

Golang 正则表达式包含两个锚字符，可以用于匹配给定正则表达式的字符串的前缀和后缀。这两个字符是

+   **插入符号字符‘^’ –** 它是一个锚字符，可以用来匹配字符串的前缀。

+   **美元字符‘$’ –** 它是一个锚字符，可以用来匹配字符串的后缀。

让我们逐个查看前缀和后缀匹配。

## **前缀匹配**

它是一个锚字符，可以用来匹配字符串的前缀。它在正则表达式的开始处使用，以确保给定的输入字符串从开头与正则表达式匹配。基本上，它匹配输入字符串的开头。

```
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

```
For abcd: true
For abc: true
For 1abc23: false
For ab: false
```

在上面的示例中，我们使用了以下正则表达式

```
^abc
```

请注意开头的插入符号字符。它将匹配任何以**“abc”**开头的字符串。尾部字符无关紧要。

它匹配所有以**“abc”**开头的字符串。这就是它能匹配的原因。

```
For abcd: true
For abc: true
```

并且不会匹配以下内容。

```
For 1abc23: false
For ab: false
```

## **后缀匹配**

它是一个锚字符，可以用来匹配字符串的后缀。它在正则表达式的末尾使用，以确保给定的输入字符串与正则表达式在末尾匹配。它匹配输入字符串的结尾。

```
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

```
For 1abc: true
For abc: true
For abcd: false
For ab: false
```

在上面的示例中，我们使用了以下正则表达式

```
abc$
```

请注意末尾的美元字符。它将匹配任何以**“abc”**结尾的字符串。开头的字符无关紧要。

它匹配所有以**“abc”**结尾的字符串。这就是它能匹配的原因。

```
For 1abc: true
For abc: true
```

并且不会匹配以下内容。

```
For abcd: false
For ab: false
```

如果我们只想匹配完整字符串，则需要在开头用**插入符号**字符和在末尾用**美元符号**字符将字符串锚定。这样我们就能进行完整字符串匹配。请参见下面的示例。

```
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

```
For abcd: false
For 1abc23: false
For abc: true
```

在上面的程序中，正则表达式是

```
^abc$
```

上面的程序为

```
abc
```

但不会匹配以下内容，因为它仅匹配完整字符串。

```
abcd
1abc23
```

此外，请查看我们的Golang进阶教程系列 – [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
