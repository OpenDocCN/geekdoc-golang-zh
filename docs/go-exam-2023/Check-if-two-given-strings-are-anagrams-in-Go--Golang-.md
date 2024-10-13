<!--yml

类别：未分类

日期：2024-10-13 06:46:23

-->

# 检查两个给定字符串在 Go (Golang) 中是否为变位词

> 来源：[`golangbyexample.com/check-two-strings-anagram-go/`](https://golangbyexample.com/check-two-strings-anagram-go/)

目录

+   概述

+   程序

## **概述**

**变位词** 是通过重新排列另一个单词或短语的字母形成的单词或短语，通常使用所有原始字母且恰好一次。例如，单词 *anagram* 本身可以重新排列为 *nagaram*，而单词 *binary* 可以变为 *brainy*，单词 *adobe* 可以变为 *abode*。

例如

```go
Input: abc, bac
Ouput: true

Input: abc, cb
Ouput: false
```

这里是实现方法的思路。创建一个字符串到整数的映射。现在

+   遍历第一个字符串并增加映射中每个字符的计数

+   遍历第二个字符串并减少映射中每个字符的计数

+   再次遍历第一个字符串，如果在映射中某个字符的计数非零，则返回 false。

+   最后返回 true

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func isAnagram(s string, t string) bool {

	lenS := len(s)
	lenT := len(t)

	if lenS != lenT {
		return false
	}

	anagramMap := make(map[string]int)

	for i := 0; i < lenS; i++ {
		anagramMap[string(s[i])]++
	}

	for i := 0; i < lenT; i++ {
		anagramMap[string(t[i])]--
	}

	for i := 0; i < lenS; i++ {
		if anagramMap[string(s[i])] != 0 {
			return false
		}
	}

	return true
}

func main() {
	output := isAnagram("abc", "bac")
	fmt.Println(output)

	output = isAnagram("abc", "bc")
	fmt.Println(output)
}
```

**输出**

```go
true
false
```

**注意：** 查看我们的 Golang 高级教程。本系列的教程内容详尽，努力涵盖所有概念及示例。本教程适合那些希望获得专业知识并深入理解 golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
