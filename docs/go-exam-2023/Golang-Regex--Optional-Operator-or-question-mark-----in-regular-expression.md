<!--yml

类别：未分类

日期：2024-10-13 06:38:08

-->

# Golang 正则表达式：正则表达式中的可选操作符或问号（?）

> 来源：[`golangbyexample.com/optional-operator-regex-golang/`](https://golangbyexample.com/optional-operator-regex-golang/)

目录

+   概述**

+   程序

+   问号操作符是非懒惰

+   关于双问号操作符

+   量词后的问号

## **概述**

问号是正则表达式中的可选操作符。这意味着它可选地匹配问号之前的字符。

例如。

```go
abcd?
```

这将匹配**“abc”**和**“abcd”**。

## **程序**

让我们来看一个相同的例子。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("abcd?")

	match := sampleRegexp.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegexp.Match([]byte("abcd"))
	fmt.Printf("For abcd: %t\n", match)

}
```

**输出**

```go
For abc: true
For abcd: true
```

通过用括号将多个字符闭合并在它们后面加上问号，也可以使多个字符变为可选。例如

```go
abc(de)?
```

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("abc(de)?")

	match := sampleRegexp.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegexp.Match([]byte("abcde"))
	fmt.Printf("For abcde: %t\n", match)

	match = sampleRegexp.Match([]byte("abcd"))
	fmt.Printf("For abcd: %t\n", match)
}
```

**输出**

```go
For abc: true
For abcde: true
For abcd: true
```

它匹配**“abc”**和**“abcde”**。

它也匹配了**“abcd”**。你一定在想为什么它会匹配**“abcd”**。

如果给定的字符串或文本包含正则表达式作为子字符串，那么它也会返回匹配。这就是为什么它会匹配，因为**“abcd”**包含**“abc”**作为子字符串，这对正则表达式来说是一个匹配。如果我们想进行完整字符串匹配，那么我们需要在正则表达式的开头和结尾使用锚字符。**插入符号**锚字符将用于开头，**美元符号**锚字符将用于结尾。

让我们来看一个相同的例子。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("^abc(de)?$")

	match := sampleRegexp.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegexp.Match([]byte("abcde"))
	fmt.Printf("For abcde: %t\n", match)

	match = sampleRegexp.Match([]byte("abcd"))
	fmt.Printf("For abcd: %t\n", match)
}
```

**输出**

```go
For abc: true
For abcde: true
For abcd: false
```

## **问号操作符是非懒惰的**

问号操作符是非懒惰或贪婪的。这意味着它将首先匹配可选模式。

在正则表达式的世界中，非懒惰（有时也称为贪婪）意味着尽可能多地匹配。而懒惰（有时也称为非贪婪）意味着仅匹配所需的部分。

例如，对于给定的正则表达式

```go
https? 
```

如果你尝试匹配以下输入字符串

```go
Better is https
```

然后有两个选项

+   匹配**http**

+   匹配**https**

然后它将总是匹配**https**而从不匹配**http**。原因在于它是非懒惰的。即使它匹配**http**，也不会停止，而是尝试匹配可选字符。如果可选字符匹配，则返回**https**，否则返回**http**。

让我们来看一个相同的例子。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("https?")

	match := sampleRegexp.Find([]byte("Better is https"))
	fmt.Printf("Match: %s\n", match)
}
```

**输出**

```go
Match: https
```

在上述程序中，我们使用了**查找**函数，该函数返回与正则表达式匹配的实际子字符串。正如你在输出中注意到的，它匹配了**“https”**而不是**“http”**，因为问号操作符是非懒惰的。

## **关于双问号操作符**

它是懒惰的。它一旦找到第一个匹配，就不再尝试进一步匹配。因此，对于上述文本，它总是返回结果为**“http”**，而从不返回**“https”**。

让我们来看一个例子。

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("https??")

	match := sampleRegexp.Find([]byte("Better is https"))
	fmt.Printf("Match: %s\n", match)
}
```

**输出**

```go
Match: http
```

## **量词后的问号**

在量词后的问号‘?’是懒惰或非贪婪的。量词可以是

+   **加号‘+’** – 一个或多个

+   **星号‘*’** – 零个或多个

见下面的示例

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("http(s+?)")

	match := sampleRegexp.Find([]byte("Better is httpsss"))
	fmt.Printf("Match: %s\n", match)

	sampleRegexp = regexp.MustCompile("http(s*?)")

	match = sampleRegexp.Find([]byte("Better is httpsss"))
	fmt.Printf("Match: %s\n", match)
}
```

**输出**

```go
Match: https
Match: http
```

在上面的程序中，我们有两种情况

+   在**加号**运算符后面加上问号

+   在**星号**运算符后面的问号

在这两种情况下，输入字符串是

```go
Better is httpsss
```

在第一种情况下，我们在正则表达式中的加号运算符后使用了问号

```go
"http(s+?)"
```

它返回的匹配是**“https”**而不是**“httpsss”**，因为在**加号**运算符后使用的问号是非贪婪的

在第二种情况下，我们在正则表达式中的星号符号后使用了问号

```go
"http(s*?)"
```

它返回的匹配是**“http”**而不是**“httpsss”**，因为在**星号**运算符后使用的问号是非贪婪的

让我们看另一个示例

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegexp := regexp.MustCompile("(a+?)(a*)")

	match := sampleRegexp.FindStringSubmatch("aaaaaaa")
	fmt.Printf("Match: %s Length: %d\n", match, len(match))

	sampleRegexp = regexp.MustCompile("(a*?)(a*)")

	match = sampleRegexp.FindStringSubmatch("aaaaaaa")
	fmt.Printf("Match: %s Length: %d\n", match, len(match))
}
```

**输出**

```go
Match: [aaaaaaa a aaaaaa] Length: 3
Match: [aaaaaaa  aaaaaaa] Length: 3
```

在上面的程序中，我们又有两种情况

+   在加号运算符后加上问号

+   在星号运算符后加上问号

在第一种情况下，我们有带有两个捕获组的正则表达式

```go
(a+?)(a*)
```

第一个捕获组给出一个匹配**‘a’**，而第二个捕获组给出其余部分。这表明问号运算符在**加号**运算符后使用是非贪婪或懒惰的

在第二种情况下，我们再次有带有两个捕获组的正则表达式

```go
(a*?)(a*)
```

第一个捕获组给出零个匹配**‘a’**，而第二个捕获组给出其余部分。这表明问号运算符在**星号**运算符后使用是非贪婪或懒惰的

这就是关于 Go 中问号运算符的全部内容。希望你喜欢这篇文章。请在评论中分享反馈。

另外，查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)


