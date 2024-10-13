<!--yml

类别: 未分类

日期: 2024-10-13 06:47:43

-->

# 电话号码字母组合程序在 Go (Golang)中

> 来源：[`golangbyexample.com/letter-combinations-phone-golang/`](https://golangbyexample.com/letter-combinations-phone-golang/)

目录

+   概述

+   程序

## **概述**

给定一个输入字符串，其中包含一些数字。数字与字母的映射类似于电话键盘。

```go
2 = either "a", "b" or "c"
3 = either "d", "e" or "f"
4 = either "g", "h" or "i"
5 = either "j", "k" or "l"
6 = either "m", "n" or "co"
7 = either "p", "q" "r" or "s"
8 = either "t", "u" or "v"
9 = either "w", "x", "y" or "z"
```

因此，给定的输入字符串仅包含数字。目标是返回所有字母组合。

示例

```go
Input: "3"
Output: [d e f]

Input: "34"
Output: [dg dh di eg eh ei fg fh fi]

Input: "345"
Output: [dgj dgk dgl dhj dhk dhl dij dik dil egj egk egl ehj ehk ehl eij eik eil fgj fgk fgl fhj fhk fhl fij fik fil]
```

## **程序**

这里是相同程序的代码。

```go
package main

import "fmt"

func letterCombinations(digits string) []string {
	if digits == "" {
		return nil
	}
	letterMap := make(map[string][]string)

	letterMap["2"] = []string{"a", "b", "c"}
	letterMap["3"] = []string{"d", "e", "f"}
	letterMap["4"] = []string{"g", "h", "i"}
	letterMap["5"] = []string{"j", "k", "l"}
	letterMap["6"] = []string{"m", "n", "o"}
	letterMap["7"] = []string{"p", "q", "r", "s"}
	letterMap["8"] = []string{"t", "u", "v"}
	letterMap["9"] = []string{"w", "x", "y", "z"}

	runeDigits := []rune(digits)
	length := len(runeDigits)

	temp := ""

	return letterCombinationsUtil(runeDigits, 0, length, temp, letterMap)

}

func letterCombinationsUtil(runeDigits []rune, start, length int, temp string, letterMap map[string][]string) []string {

	if start == length {
		return []string{temp}
	}

	currentDigit := string(runeDigits[start])

	letters := letterMap[currentDigit]

	final := make([]string, 0)
	for _, val := range letters {
		t := temp + val
		output := letterCombinationsUtil(runeDigits, start+1, length, t, letterMap)
		final = append(final, output...)
	}
	return final
}

func main() {

	output := letterCombinations("3")
	fmt.Println(output)

	output = letterCombinations("34")
	fmt.Println(output)

	output = letterCombinations("345")
	fmt.Println(output)
}
```

**输出**

```go
[d e f]
[dg dh di eg eh ei fg fh fi]
[dgj dgk dgl dhj dhk dhl dij dik dil egj egk egl ehj ehk ehl eij eik eil fgj fgk fgl fhj fhk fhl fij fik fil]
```

**注意：** 查看我们的 Golang 高级教程。本系列教程详尽，我们努力覆盖所有概念并提供示例。本教程适合那些希望获得专业知识和扎实理解 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章就是为你准备的 – [所有设计模式在 Golang 中的实现](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
