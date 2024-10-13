<!--yml

分类：未分类

日期：2024-10-13 06:37:33

-->

# Golang 正则表达式：回溯引用

> 来源：[`golangbyexample.com/golang-regex-backreferences/`](https://golangbyexample.com/golang-regex-backreferences/)

目录

+   概述

+   程序

    +   第一个示例

    +   第二个示例

+   替换匹配字符串

## **概述**

Golang 正则表达式包[regexp](https://golang.org/pkg/regexp/)使用[re2 引擎](https://swtch.com/~rsc/regexp/regexp3.html)，它不支持回溯引用。您可以在这里查看

[`github.com/google/re2/wiki/Syntax`](https://github.com/google/re2/wiki/Syntax)

它确实提到它不支持回溯引用。

**然而，还有另一个可用的 golang 包，它使用 libpcre++、Perl 正则表达式，并且支持回溯引用。**

```go
https://github.com/glenn-brown/golang-pkg-pcre/tree/master/src/pkg/pcre
```

## **程序**

所以让我们看看在 golang 中使用这个**pcre**包的回溯引用示例。

### **第一个示例**

假设我们想匹配数字的重复。有效输入是

```go
1111
888888888
444
```

匹配同样内容的正则表达式将是

```go
(\d)\1+
```

让我们分析一下这个正则表达式

+   **(\d)** – 匹配单个数字。单个数字被括号包围，因此它作为捕获组。

+   **\1** – 通过捕获组回溯引用第一个子匹配。因此它将引用第一个数字

+   **+** – 前一个数字出现一次或多次

同样的程序

```go
package main

import (
	"fmt"

	"github.com/glenn-brown/golang-pkg-pcre/src/pkg/pcre"
)

func main() {
	regex := pcre.MustCompile(`(\d)\1+`, 0)

	matches := regex.MatcherString("1111", 0).Matches()
	fmt.Println("For 1111 : ", matches)

	matches = regex.MatcherString("88888888", 0).Matches()
	fmt.Println("For 88888888 : ", matches)

	matches = regex.MatcherString("444", 0).Matches()
	fmt.Println("For 444 : ", matches)

	matches = regex.MatcherString("123", 0).Matches()
	fmt.Println("For 123 : ", matches)
}
```

**输出**

```go
For 1111 :  true
For 88888888 :  true
For 444 :  true
For 123 :  false
```

如预期，它能匹配数字的重复

```go
1111
888888888
444
```

并且它不会匹配以下内容，因为这不是重复

```go
123
```

### **第二个示例**

假设我们想匹配以冒号分隔的单词重复。有效输入是

```go
John:John
The names are Simon:Simon
```

匹配同样内容的正则表达式将是

```go
(\w+):\1
```

让我们分析一下这个正则表达式

+   **(\w+)** – 匹配一个字符超过一个的单词。它被括号包围，因此它作为捕获组。

+   **\1** – 通过捕获组回溯引用第一个子匹配。因此它将引用匹配的单词

同样的程序

```go
package main

import (
	"fmt"

	"github.com/glenn-brown/golang-pkg-pcre/src/pkg/pcre"
)

func main() {
	regex := pcre.MustCompile(`(\w+):\1`, 0)

	matches := regex.MatcherString("John:John", 0).Matches()
	fmt.Println("For John:John: ", matches)

	matches = regex.MatcherString("The names are Simon:Simon", 0).Matches()
	fmt.Println("For The names are Simon:Simon: ", matches)

	matches = regex.MatcherString("John:Simon", 0).Matches()
	fmt.Println("For John:Simon: ", matches)

}
```

**输出**

```go
For John:John:  true
For The names are Simon:Simon:  true
For John:Simon:  false
```

如预期，它能匹配包含重复单词子字符串的字符串

```go
John:John
The names are Simon:Simon
```

并且它不会匹配以下内容，因为它不包含单词的重复

```go
John:Simon
```

## **替换匹配字符串**

**pcre**包还提供了替换匹配字符串的功能。以下是相同的示例。

```go
package main

import (
	"fmt"

	"github.com/glenn-brown/golang-pkg-pcre/src/pkg/pcre"
)

func main() {
	regex := pcre.MustCompile(`(\d)\1+`, 0)

	input := "The number is 91-88888888"

	result := regex.ReplaceAll([]byte(input), []byte("redacted"), 0)
	fmt.Println("result: ", string(result))
}
```

**输出**

```go
result:  The number is 91-redacted
```

在上面的例子中，我们有一个包含回溯引用的正则表达式，用于匹配数字的重复。然后我们使用**ReplaceAll**方法对该数字的重复进行修饰

```go
result := regex.ReplaceAll([]byte(input), []byte("redacted"), 0)
```

如预期，输出中数字的重复被正确修饰

```go
result:  The number is 91-redacted
```

希望您喜欢这个教程。请在评论中分享反馈

另外，查看我们的 Golang 进阶教程系列 – [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [反向引用](https://golangbyexample.com/tag/backreference/)*   [去](https://golangbyexample.com/tag/go/)*   [Go 语言](https://golangbyexample.com/tag/golang/)*   [正则表达式](https://golangbyexample.com/tag/regex/)*
