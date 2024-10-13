<!--yml

类别：未分类

日期：2024-10-13 06:52:05

-->

# 通过删除在 Go (Golang) 中查找字典中最长单词的程序

> 来源：[https://golangbyexample.com/longest-word-dictionary-go/](https://golangbyexample.com/longest-word-dictionary-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

给定一个字符串和一个单词字典。目标是找到字典中作为子序列出现在给定字符串中的最长单词。如果可能的结果数量超过 1，则返回字典中按字典顺序最小的最长单词。

**示例 1**

```go
s = "mbacnago", dictionary = ["ale","mango","monkey","plea"]
Output: "mango"
```

**示例 2**

```go
s = "mbacnago", dictionary = ["ba","ag"]
Output: "ag"
```

# **程序**

下面是相同的程序

```go
package main

import (
	"fmt"
	"sort"
)

func findLongestWord(s string, dictionary []string) string {
	sort.Slice(dictionary, func(i, j int) bool {
		lenI := len(dictionary[i])
		lenJ := len(dictionary[j])

		if lenI == lenJ {
			return dictionary[i] < dictionary[j]
		}

		return lenI > lenJ
	})

	lenS := len(s)

	for i := 0; i < len(dictionary); i++ {
		if isSubstring(s, dictionary[i], lenS) {
			return dictionary[i]
		}
	}

	return ""
}

func isSubstring(s string, sub string, lenS int) bool {
	lenSub := len(sub)

	if lenSub == 0 {
		return true
	}

	if lenSub > lenS {
		return false
	}

	for i, j := 0, 0; i < lenS && j < lenSub; {
		if i+lenSub-j-1 >= lenS {
			return false
		}
		if s[i] == sub[j] {
			j++
		}
		if j == lenSub {
			return true
		}
		i++
	}

	return false
}

func main() {
	output := findLongestWord("mbacnago", []string{"ale", "mango", "monkey", "plea"})
	fmt.Println(output)

	output = findLongestWord("mbacnago", []string{"ba", "ag"})
	fmt.Println(output)

}
```

**输出**

```go
mango
ag
```

**注意：** 请查看我们的 Golang 高级教程。此系列教程内容详尽，我们尽力涵盖所有概念及示例。本教程适合希望获得专业知识和对 Golang 有扎实理解的学习者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对理解所有设计模式在 Golang 中的实现感兴趣，那么这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

同时，可以在这里查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*
