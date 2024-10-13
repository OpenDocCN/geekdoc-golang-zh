<!--yml

类别：未分类

日期：2024-10-13 06:48:29

-->

# 计算字符串中最后一个单词长度的 Go 程序 (Golang)

> 来源：[`golangbyexample.com/length-last-word-golang/`](https://golangbyexample.com/length-last-word-golang/)

目录

**   概述

+   程序*  *## **概述**

目标是找到给定字符串中最后一个单词的长度

示例

```go
Input: "computer science"
Output: 7

The last word is science and its length is 7

Input: "computer science is a subject "
Output: 7

The last word is subject and ts length is 7

Input: " "
Output: 0

There is no last word hence answer is zero
```

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func lengthOfLastWord(s string) int {
	lenS := len(s)

	lenLastWord := 0
	for i := lenS - 1; i >= 0; {
		for i >= 0 && string(s[i]) == " " {
			i--
		}
		if i < 0 {
			return 0
		}

		for i >= 0 && string(s[i]) != " " {
			//fmt.Println(i)
			//fmt.Println(string(s[i]))
			i--
			lenLastWord++
		}

		return lenLastWord
	}

	return 0
}

func main() {
	length := lengthOfLastWord("computer science")
	fmt.Println(length)

	length = lengthOfLastWord("computer science is a subject")
	fmt.Println(length)

	length = lengthOfLastWord("  ")
	fmt.Println(length)
}
```

**输出**

```go
7
7
0
```

**注意：** 查看我们的 Golang 高级教程。该系列教程详细而全面，我们尝试用示例覆盖所有概念。本教程适合那些希望获得专业知识和扎实理解 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
