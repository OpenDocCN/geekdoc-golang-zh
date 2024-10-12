<!--yml
category: 未分类
date: 2024-10-13 06:44:26
-->

# Group anagrams together program in Go (Golang)

> 来源：[https://golangbyexample.com/group-anagrams-together-go/](https://golangbyexample.com/group-anagrams-together-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Given an array of strings, write a program to group all anagrams together.  From Wikipedia

An **anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once. For example, the word *anagram* itself can be rearranged into *nagaram*, also the word *binary* into *brainy*^, and the word *adobe* into the *abode*.
For eg

```
Input: ["art", "tap", "rat", "pat", "tar","arm"]
Output: [["art", "rat", "tar"], ["tap", "pat"], ["arm"]]
```

Below will the strategy.

*   Duplicate the original array. Sort each string in the duplicate array. After sorting the duplicate array will look like this

```
["art", "apt", "art", "apt", "art", "arm"]
```

*   Create a map to store the output

```
var output map[string][]int
```

*   Build a trie for the above duplicate array with all strings sorted. Update the map above after inserting each element. Map should look like as below for “art” as art has its anagrams at 0,2 and 5 positions in the original array.

```
map["art"] = [0,2,4]
```

*   Iterate over the map and print the output by indexing in the input array of strings

## **Program**

Below is the program for the same

```
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

**Output**

```
[[art rat tar] [tap pat] [arm]]
[[]]
[[a]]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*