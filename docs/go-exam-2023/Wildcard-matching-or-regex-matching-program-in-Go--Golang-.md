<!--yml

分类：未分类

日期：2024-10-13 06:44:04

-->

# Go（Golang）中的通配符匹配或正则表达式匹配程序。

> 来源：[`golangbyexample.com/wildcard-matching-golang/`](https://golangbyexample.com/wildcard-matching-golang/)

目录

**   概述

+   递归解决方案

+   动态规划解决方案 *  *## **概述**

我们给定一个输入正则表达式和一个输入字符串。正则表达式可以包含两个特殊字符。

+   **星号 ‘*’ –** 星号匹配零个或多个字符。

+   **问号 ‘?’ –** 它匹配任何字符。

目标是找出给定的输入字符串是否与正则表达式匹配。

例如

```go
Input String: aa
Regex Sring: aa
Output: true

Input String: ab
Regex Sring: a?
Output: true

Input String: aaaa
Regex Sring: *
Output: true

Input String: aa
Regex Sring: a
Output: false
```

下面是相应的递归解决方案

## **递归解决方案**

在递归解决方案中

+   如果我们遇到星号 *，那么我们有两种情况。我们忽略模式中的 * 字符，继续处理模式中的下一个字符。另一种情况是我们在输入字符串中移动一个字符，假设 * 至少匹配一个字符。基本上检查 **(inputIndex, patternIndex+1)** 和 **(inputIndex+1, patternIndex)** 的匹配。如果它们中的任何一个返回真，则输入字符串与正则表达式匹配。

+   如果我们遇到问号 ?，那么我们简单地继续进行 **(inputIndex+1, patternIndex+1)**。

+   如果我们遇到一个简单字符，则我们在输入字符串和模式中都简单地继续，即我们继续进行 **(inputIndex+1, patternIndex+1)**。

这是程序

```go
package main

import "fmt"

func main() {
	output := isMatch("aa", "aa")
	fmt.Println(output)

	output = isMatch("aaaa", "*")
	fmt.Println(output)

	output = isMatch("ab", "a?")
	fmt.Println(output)

	output = isMatch("adceb", "*a*b")
	fmt.Println(output)

	output = isMatch("aa", "a")
	fmt.Println(output)

	output = isMatch("mississippi", "m??*ss*?i*pi")
	fmt.Println(output)

	output = isMatch("acdcb", "a*c?b")
	fmt.Println(output)
}

func isMatch(s string, p string) bool {
	runeInputArray := []rune(s)
	runePatternArray := []rune(p)
	if len(runeInputArray) > 0 && len(runePatternArray) > 0 {
		if runePatternArray[len(runePatternArray)-1] != '*' && runePatternArray[len(runePatternArray)-1] != '?' && runeInputArray[len(runeInputArray)-1] != runePatternArray[len(runePatternArray)-1] {
			return false
		}
	}
	return isMatchUtil([]rune(s), []rune(p), 0, 0, len([]rune(s)), len([]rune(p)))
}

func isMatchUtil(input, pattern []rune, inputIndex, patternIndex int, inputLength, patternLength int) bool {

	if inputIndex == inputLength && patternIndex == patternLength {
		return true
	} else if patternIndex == patternLength {
		return false
	} else if inputIndex == inputLength {
		if pattern[patternIndex] == '*' && restPatternStar(pattern, patternIndex+1, patternLength) {
			return true
		} else {
			return false
		}
	}

	if pattern[patternIndex] == '*' {
		return isMatchUtil(input, pattern, inputIndex, patternIndex+1, inputLength, patternLength) ||
			isMatchUtil(input, pattern, inputIndex+1, patternIndex, inputLength, patternLength)

	}

	if pattern[patternIndex] == '?' {
		return isMatchUtil(input, pattern, inputIndex+1, patternIndex+1, inputLength, patternLength)
	}

	if inputIndex < inputLength {
		if input[inputIndex] == pattern[patternIndex] {
			return isMatchUtil(input, pattern, inputIndex+1, patternIndex+1, inputLength, patternLength)
		} else {
			return false
		}
	}

	return false

}

func restPatternStar(pattern []rune, patternIndex int, patternLength int) bool {
	for patternIndex < patternLength {
		if pattern[patternIndex] != '*' {
			return false
		}
		patternIndex++
	}

	return true

}
```

**输出**

```go
true
true
true
true
false
false
false
```

## **动态规划解决方案**

上述程序不是优化解决方案，因为子问题被重复求解。这个问题也可以用动态规划解决。

创建一个名为 **isMatchingMatrix** 的二维矩阵，其中

**isMatchingMatrix[i][j]** 将为真，如果输入字符串的前 **i** 个字符与模式的前 **j** 个字符匹配。

```go
If both input and pattern is empty
isMatchingMatrix[0][0] = true

If pattern is empty 
isMatchingMatrix[i][0] = fasle

If the input string is empty 
isMatchingMatrix[0][j] = isMatchingMatrix[0][j - 1] if pattern[j – 1] is '*'
```

下面是相应的程序。

```go
package main

import "fmt"

func main() {
	output := isMatch("aa", "aa")
	fmt.Println(output)

	output = isMatch("aaaa", "*")
	fmt.Println(output)

	output = isMatch("ab", "a?")
	fmt.Println(output)

	output = isMatch("adceb", "*a*b")
	fmt.Println(output)

	output = isMatch("aa", "a")
	fmt.Println(output)

	output = isMatch("mississippi", "m??*ss*?i*pi")
	fmt.Println(output)

	output = isMatch("acdcb", "a*c?b")
	fmt.Println(output)
}

func isMatch(s string, p string) bool {

	runeInput := []rune(s)
	runePattern := []rune(p)

	lenInput := len(runeInput)
	lenPattern := len(runePattern)

	isMatchingMatrix := make([][]bool, lenInput+1)

	for i := range isMatchingMatrix {
		isMatchingMatrix[i] = make([]bool, lenPattern+1)
	}

	isMatchingMatrix[0][0] = true
	for i := 1; i < lenInput; i++ {
		isMatchingMatrix[i][0] = false
	}

	if lenPattern > 0 {
		if runePattern[0] == '*' {
			isMatchingMatrix[0][1] = true
		}
	}

	for j := 2; j <= lenPattern; j++ {
		if runePattern[j-1] == '*' {
			isMatchingMatrix[0][j] = isMatchingMatrix[0][j-1]
		}

	}

	for i := 1; i <= lenInput; i++ {
		for j := 1; j <= lenPattern; j++ {

			if runePattern[j-1] == '*' {
				isMatchingMatrix[i][j] = isMatchingMatrix[i-1][j] || isMatchingMatrix[i][j-1]
			}

			if runePattern[j-1] == '?' || runeInput[i-1] == runePattern[j-1] {
				isMatchingMatrix[i][j] = isMatchingMatrix[i-1][j-1]
			}
		}
	}

	return isMatchingMatrix[lenInput][lenPattern]
}
```

**输出**

```go
true
true
true
true
false
false
false
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，旨在涵盖所有概念和示例。此教程适合希望获得专业知识和深入理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
