<!--yml
category: 未分类
date: 2024-10-13 06:46:23
-->

# Check if two given strings are anagrams in Go (Golang)

> 来源：[https://golangbyexample.com/check-two-strings-anagram-go/](https://golangbyexample.com/check-two-strings-anagram-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An **anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once. For example, the word *anagram* itself can be rearranged into *nagaram*, also the word *binary* into *brainy*^, and the word *adobe* into the *abode*.

For example

```
Input: abc, bac
Ouput: true

Input: abc, cb
Ouput: false
```

Here is the idea of how to do it. Create a map of string to int. Now

*   Traverse the first string and increase the count of each character in the map

*   Traverse the second string and decrease the count of each character in the map

*   Traverse the first string again and if for any character the count is non-zero in the map then return false.

*   In the end return true

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func isAnagram(s string, t string) bool {

	lenS := len(s)
	lenT := len(t)

	if lenS != lenT {
		return false
	}

	anagramMap := make(map[string]int)

	for i := 0; i < lenS; i++ {
		anagramMap[string(s[i])]++
	}

	for i := 0; i < lenT; i++ {
		anagramMap[string(t[i])]--
	}

	for i := 0; i < lenS; i++ {
		if anagramMap[string(s[i])] != 0 {
			return false
		}
	}

	return true
}

func main() {
	output := isAnagram("abc", "bac")
	fmt.Println(output)

	output = isAnagram("abc", "bc")
	fmt.Println(output)
}
```

**Output**

```
true
false
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*