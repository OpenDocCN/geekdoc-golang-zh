<!--yml

类别: 未分类

日期: 2024-10-13 06:37:12

-->

# Golang 正则表达式: Go（Golang）中的大小写不敏感正则表达式匹配

> 来源：[https://golangbyexample.com/case-insensitive-regex-golang/](https://golangbyexample.com/case-insensitive-regex-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

在golang中，正则表达式匹配的默认行为是大小写敏感的。但通过在正则表达式开头添加一组标志，可以更改默认行为。我们需要在正则表达式开头添加的标志是：

```
(?i)
```

标志**‘i’**用于指示正则表达式将不区分大小写。

这里是大小写敏感和不敏感正则表达式的示例。

**大小写敏感正则表达式**

```
abc
```

**大小写不敏感正则表达式**

```
(?i)abc
```

## **程序**

让我们看看相同的程序。

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("(?i)abc")

	match := sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegex.Match([]byte("ABC"))
	fmt.Printf("For ABC: %t\n", match)
}
```

**输出**

```
For abc: true
For ABC: true
```

注意上面程序中的正则表达式。我们用**(?i)**前缀正则表达式，以指示该正则表达式将不区分大小写。

```
(?i)abc
```

从输出中我们可以注意到，它对文本**“abc”**和**“ABC”**都给出了正确的匹配。

如果我们移除前缀标志，那么它将会对**“ABC”**产生错误匹配。

```
package main

import (
	"fmt"
	"regexp"
)

func main() {
	sampleRegex := regexp.MustCompile("abc")

	match := sampleRegex.Match([]byte("abc"))
	fmt.Printf("For abc: %t\n", match)

	match = sampleRegex.Match([]byte("ABC"))
	fmt.Printf("For ABC: %t\n", match)
}
```

**输出**

```
For abc: true
For ABC: false
```

注意上面程序中的正则表达式。我们没有用**(?i)**前缀正则表达式。因此它将回退到默认的大小写敏感行为。

```
abc
```

从输出中我们可以注意到，它对文本**“abc”**给出了正确匹配，但对文本**“ABC”**没有匹配。

这就是关于golang中大小写不敏感正则表达式匹配的全部内容。希望你喜欢这篇文章。

请在评论中分享反馈。此外，查看我们的Golang高级教程系列 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
