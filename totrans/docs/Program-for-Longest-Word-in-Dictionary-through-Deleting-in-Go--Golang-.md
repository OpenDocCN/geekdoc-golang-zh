<!--yml
category: 未分类
date: 2024-10-13 06:52:05
-->

# Program for Longest Word in Dictionary through Deleting in Go (Golang)

> 来源：[https://golangbyexample.com/longest-word-dictionary-go/](https://golangbyexample.com/longest-word-dictionary-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

A string and a dictionary of words are given. The objective is to find the longest word in the dictionary which is present as a subsequence in the given string. If the number of possible results is more than 1 then return the longest word with the smallest lexicographical order.

**Example 1**

```
s = "mbacnago", dictionary = ["ale","mango","monkey","plea"]
Output: "mango"
```

**Example 2**

```
s = "mbacnago", dictionary = ["ba","ag"]
Output: "ag"
```

# **Program**

Below is the program for the same

```
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

**Output**

```
mango
ag
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*