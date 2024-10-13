<!--yml

类别：未分类

日期：2024-10-13 06:47:38

-->

# 在Go中反转句子中的单词 (Golang)

> 来源：[https://golangbyexample.com/reverse-words-sentence-golang/](https://golangbyexample.com/reverse-words-sentence-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是反转给定句子中的单词

示例

```go
Input: "hello world"
Output: "word hello"
```

另一个例子。如果输入包含一个单词，则返回该单词。

```go
Input: "hello"
Output: "hello"
```

这是策略

+   首先，我们反转整个字符串。因此，对于“hello world”来说，它变成了

```go
"dlrow olleh"
```

+   然后我们反转每个单词

```go
"world hello"
```

+   我们还需要处理开头或结尾的多余空格。

## **程序**

这是相应的程序。

```go
package main

import (
	"fmt"
	"regexp"
	"strings"
)

func reverseWords(s string) string {

	runeArray := []rune(s)
	length := len(runeArray)

	reverseRuneArray := reverse(runeArray)

	for i := 0; i < length; {
		for i < length && string(reverseRuneArray[i]) == " " {
			i++
		}
		if i == length {
			break
		}
		wordStart := i

		for i < length && string(reverseRuneArray[i]) != " " {
			i++
		}

		wordEnd := i - 1

		reverseRuneArray = reverseIndex(reverseRuneArray, wordStart, wordEnd)

	}

	noSpaceString := strings.TrimSpace(string(reverseRuneArray))
	space := regexp.MustCompile(`\s+`)
	return space.ReplaceAllString(noSpaceString, " ")
}

func reverse(s []rune) []rune {
	length := len(s)
	start := 0
	end := length - 1
	for start < end {
		s[start], s[end] = s[end], s[start]
		start++
		end--
	}
	return s
}

func reverseIndex(s []rune, i, j int) []rune {

	start := i
	end := j
	for start < end {
		s[start], s[end] = s[end], s[start]
		start++
		end--
	}
	return s
}

func main() {
	output := reverseWords("hello world")
	fmt.Println(output)

	output = reverseWords("hello")
	fmt.Println(output)
}
```

**输出**

```go
world hello
hello
```

**注意：** 请查看我们的Golang高级教程。本系列的教程内容详尽，我们尽力涵盖所有概念和示例。本教程适合那些希望获得专业知识和深入理解Golang的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)*
