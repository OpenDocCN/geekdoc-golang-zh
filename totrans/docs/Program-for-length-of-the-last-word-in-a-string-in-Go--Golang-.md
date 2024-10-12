<!--yml
category: 未分类
date: 2024-10-13 06:48:29
-->

# Program for length of the last word in a string in Go (Golang)

> 来源：[https://golangbyexample.com/length-last-word-golang/](https://golangbyexample.com/length-last-word-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the length of the last word in a given string

Example

```
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

## **Program**

Here is the program for the same.

```
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

**Output**

```
7
7
0
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*