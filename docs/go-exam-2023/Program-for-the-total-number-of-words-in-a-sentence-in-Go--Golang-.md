<!--yml

分类：未分类

日期：2024-10-13 06:49:17

-->

# 在 Go（Golang）中计算句子中单词总数的程序

> 来源：[`golangbyexample.com/number-words-sentence-golang/`](https://golangbyexample.com/number-words-sentence-golang/)

目录

+   概述

+   程序

## **概述**

给定一个句子，找出其中的单词数量。句子中的每个单词只包含英语字母。

示例

```go
Input: "Hello World"
Output: 2

Input: "This is hat"
Output: 3
```

## **程序**

这是相同的程序。

```go
package main

import "fmt"

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
	output := countW("Hello World")
	fmt.Println(output)

	output = countW("This is hat")
	fmt.Println(output)
}
```

**输出**

```go
2
3
```

**注意：** 请查看我们的 Golang 高级教程。该系列教程内容丰富，我们尝试用例子覆盖所有概念。本教程适合希望获得专业知识和对 Golang 有深入理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对如何在 Golang 中实现所有设计模式感兴趣，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


