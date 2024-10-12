<!--yml
category: 未分类
date: 2024-10-13 06:50:09
-->

# Longest Common Prefix in a set of strings in Go (Golang)

> 来源：[https://golangbyexample.com/longest-common-prefix-golang/](https://golangbyexample.com/longest-common-prefix-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An array of strings is given. The objective is to find the longest common prefix from that array of strings.  It should output an empty string if there is no common prefix.

Example 1

```
Input: ["fan", "fat", "fame"]
Output: "fa"
```

Example 2

```
Input: ["bat", "van", "cat"]
Output: ""
```

## **Program**

Below is the program for the same

```
package main

import "fmt"

func longestCommonPrefix(strs []string) string {
	lenStrs := len(strs)

	if lenStrs == 0 {
		return ""
	}

	firstString := strs[0]

	lenFirstString := len(firstString)

	commonPrefix := ""
	for i := 0; i < lenFirstString; i++ {
		firstStringChar := string(firstString[i])
		match := true
		for j := 1; j < lenStrs; j++ {
			if (len(strs[j]) - 1) < i {
				match = false
				break
			}

			if string(strs[j][i]) != firstStringChar {
				match = false
				break
			}

		}

		if match {
			commonPrefix += firstStringChar
		} else {
			break
		}
	}

	return commonPrefix
}

func main() {
	output := longestCommonPrefix([]string{"fan", "fat", "fame"})
	fmt.Println(output)

	output = longestCommonPrefix([]string{"bat", "van", "cat"})
	fmt.Println(output)
}
```

**Output:**

```
"fa"
""
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*