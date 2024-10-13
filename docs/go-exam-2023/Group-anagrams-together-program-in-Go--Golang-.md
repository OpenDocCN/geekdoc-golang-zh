<!--yml

类别：未分类

日期：2024-10-13 06:44:26

-->

# 在 Go (Golang)中将字母异位词组合在一起的程序

> 来源：[`golangbyexample.com/group-anagrams-together-go/`](https://golangbyexample.com/group-anagrams-together-go/)

目录

+   概述

+   程序

## **概述**

给定一个字符串数组，编写一个程序将所有字母异位词组合在一起。来自维基百科

**字母异位词**是通过重新排列不同单词或短语的字母形成的单词或短语，通常使用所有原始字母一次。例如，单词*anagram*本身可以重新排列为*nagaram*，单词*binary*可以变为*brainy*^，单词*adobe*可以变为*abode*。

例如

```go
Input: ["art", "tap", "rat", "pat", "tar","arm"]
Output: [["art", "rat", "tar"], ["tap", "pat"], ["arm"]]
```

以下是策略。

+   复制原始数组。对副本数组中的每个字符串进行排序。排序后的副本数组将如下所示

```go
["art", "apt", "art", "apt", "art", "arm"]
```

+   创建一个映射以存储输出

```go
var output map[string][]int
```

+   为上述副本数组构建一个前缀树，所有字符串都已排序。插入每个元素后更新上述映射。对于“art”，映射应如下所示，因为 art 在原始数组中的位置为 0、2 和 5。

```go
map["art"] = [0,2,4]
```

+   遍历映射并通过在输入字符串数组中索引打印输出

## **程序**

以下是相应的程序

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	strs := []string{"art", "tap", "rat", "pat", "tar", "arm"}
	output := groupAnagrams(strs)
	fmt.Println(output)

	strs = []string{""}
	output = groupAnagrams(strs)
	fmt.Println(output)

	strs = []string{"a"}
	output = groupAnagrams(strs)
	fmt.Println(output)
}

type sortRune []rune

func (s sortRune) Swap(i, j int) {
	s[i], s[j] = s[j], s[i]
}

func (s sortRune) Less(i, j int) bool {
	return s[i] < s[j]
}

func (s sortRune) Len() int {
	return len(s)
}

func groupAnagrams(strs []string) [][]string {

	anagramMap := make(map[string][]int)
	var anagrams [][]string
	trie := &trie{root: &trieNode{}}

	lenStrs := len(strs)

	var strsDup []string

	for i := 0; i < lenStrs; i++ {
		runeCurrent := []rune(strs[i])
		sort.Sort(sortRune(runeCurrent))
		strsDup = append(strsDup, string(runeCurrent))
	}

	for i := 0; i < lenStrs; i++ {
		anagramMap = trie.insert(strsDup[i], i, anagramMap)
	}

	for _, value := range anagramMap {
		var combinedTemp []string
		for i := 0; i < len(value); i++ {
			combinedTemp = append(combinedTemp, strs[value[i]])
		}
		anagrams = append(anagrams, combinedTemp)
	}

	return anagrams
}

type trieNode struct {
	isWord    bool
	childrens [26]*trieNode
}

type trie struct {
	root *trieNode
}

func (t *trie) insert(input string, wordIndex int, anagramMap map[string][]int) map[string][]int {
	inputLen := len(input)
	current := t.root

	for i := 0; i < inputLen; i++ {
		index := input[i] - 'a'
		if current.childrens[index] == nil {
			current.childrens[index] = &trieNode{}
		}
		current = current.childrens[index]
	}
	current.isWord = true
	if anagramMap[input] == nil {
		anagramMap[input] = []int{wordIndex}
	} else {
		anagramMap[input] = append(anagramMap[input], wordIndex)
	}
	return anagramMap
}
```

**输出**

```go
[[art rat tar] [tap pat] [arm]]
[[]]
[[a]]
```

**注意：** 查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试覆盖所有概念及示例。本教程适合希望获得专业知识和对 golang 有深入理解的学习者 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


