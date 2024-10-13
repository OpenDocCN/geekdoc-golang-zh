<!--yml

分类：未分类

日期：2024-10-13 06:41:56

-->

# 在 Go (Golang)中找到最长无重复字符的子字符串程序。

> 来源：[`golangbyexample.com/longest-substring-without-repeating-characters-golang/`](https://golangbyexample.com/longest-substring-without-repeating-characters-golang/)

目录

+   概述

+   程序

## **概述**

给定一个字符串，我们必须找到其中最长的无重复字符的子字符串。例如，如果字符串是

```go
abbabcda
```

那么答案将是**“abcd”**，长度应为 4。

我们使用哈希表和三个变量。

+   哈希表跟踪任何字符的最后索引位置。

+   longestSubstringLength – 这个变量存储到目前为止最长的无重复字符的子字符串长度。

+   currentSubstringLength – 这个变量存储无重复字符的当前子字符串长度。

+   start – 这表示当前无重复字符子字符串的起始位置。

我们遍历字符串，并检查当前字符的哈希表。在以下两个条件下，我们简单地增加**currentSubstringLength**。

+   如果当前字符的条目不存在，则说明当前字符之前未出现过。

+   如果条目存在并且当前字符之前已出现，但不属于当前长度。

否则

+   我们重置**起始**位置和**currentSubstringLength**以将当前字符包括在当前长度中。在重置之前，我们检查**currentSubstringLength**是否大于**longestSubstringLength**。如果是，则将**longestSubstringLength**设置为**currentSubstringLength**。

让我们看一下相应的程序。

## **程序**

```go
package main

import "fmt"

func main() {
	len := lengthOfLongestSubstring("abbabcda")
	fmt.Println(len)
}

func lengthOfLongestSubstring(s string) int {
	charLastIndex := make(map[rune]int)

	longestSubstringLength := 0
	currentSubstringLength := 0
	start := 0

	for index, character := range s {
		lastIndex, ok := charLastIndex[character]
		if !ok || lastIndex < index-currentSubstringLength {
			currentSubstringLength++
		} else {
			if currentSubstringLength > longestSubstringLength {
				longestSubstringLength = currentSubstringLength
			}
			start = lastIndex + 1
			currentSubstringLength = index - start + 1
		}
		charLastIndex[character] = index
	}
	if currentSubstringLength > longestSubstringLength {
		longestSubstringLength = currentSubstringLength
	}
	return longestSubstringLength
}
```

**输出**

```go
4
```*
