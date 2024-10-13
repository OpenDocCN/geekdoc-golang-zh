<!--yml

分类：未分类

日期：2024-10-13 06:49:21

-->

# Go (Golang) 中一组句子中的最大单词数

> 来源：[`golangbyexample.com/maximum-words-group-sentences-go/`](https://golangbyexample.com/maximum-words-group-sentences-go/)

目录

**   概述

+   程序*  *## **概述**

给定一组句子。找到在该组句子中出现的最大单词数。

示例

```go
Input: ["Hello World", "This is hat]
Output: 3

Input: ["Hello World", "This is hat", "The cat is brown"]
Output: 4
```

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func mostWordsFound(sentences []string) int {
	lenSentences := len(sentences)

	max := 0
	for i := 0; i < lenSentences; i++ {
		countWord := countW(sentences[i])
		if countWord > max {
			max = countWord
		}
	}

	return max
}

func countW(s string) int {

	lenS := len(s)
	numWords := 0

	for i := 0; i < lenS; {
		for i < lenS && string(s[i]) == " " {
			i++
		}

		if i < lenS {
			numWords++
		}

		for i < lenS && string(s[i]) != " " {
			i++
		}
	}

	return numWords
}

func main() {
	output := mostWordsFound([]string{"Hello World", "This is hat"})
	fmt.Println(output)

	output = mostWordsFound([]string{"Hello World", "This is hat", "The cat is brown"})
	fmt.Println(output)
}
```

**输出**

```go
3
4
```

**注意：** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试覆盖所有概念并附带示例。本教程适合希望掌握和深入理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是这样，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [前往](https://golangbyexample.com/tag/go/)*
