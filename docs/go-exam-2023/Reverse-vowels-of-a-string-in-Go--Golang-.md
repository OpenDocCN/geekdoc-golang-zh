<!--yml

类别：未分类

日期：2024-10-13 06:50:52

-->

# 在 Go (Golang)中反转字符串的元音

> 来源：[`golangbyexample.com/reverse-vowels-string-golang/`](https://golangbyexample.com/reverse-vowels-string-golang/)

目录

**   概述

+   程序*  *# **概述**

给定一个字符串。目标是反转该字符串中的所有元音。

**示例 1**

```go
Input: "simple"
Output: "sempli"
```

**示例 2**

```go
Input: "complex"
Output: "cemplox"
```

# **程序**

以下是相应的程序

```go
package main

import "fmt"

func reverseVowels(s string) string {
	runeS := []rune(s)
	lenS := len(runeS)

	for i, j := 0, lenS-1; i < j; {
		for i < j {
			if !vowel(runeS[i]) {
				i++
			} else {
				break
			}
		}
		if i == j {
			break
		}
		for i < j {
			if !vowel(runeS[j]) {
				j--
			} else {
				break
			}
		}

		if i == j {
			break
		}

		runeS[i], runeS[j] = runeS[j], runeS[i]
		i++
		j--

	}

	return string(runeS)

}

func vowel(s rune) bool {
	if s == 'a' || s == 'e' || s == 'i' || s == 'o' || s == 'u' {
		return true
	}

	if s == 'A' || s == 'E' || s == 'I' || s == 'O' || s == 'U' {
		return true
	}

	return false
}

func main() {
	output := reverseVowels("simple")
	fmt.Println(output)

	output = reverseVowels("complex")
	fmt.Println(output)

}
```

**输出：**

```go
sempli
cemplox
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽量涵盖所有概念和示例。此教程适合希望获得 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

另外，请查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)*
