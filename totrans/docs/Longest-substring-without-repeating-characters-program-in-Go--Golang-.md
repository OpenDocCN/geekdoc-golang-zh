<!--yml
category: 未分类
date: 2024-10-13 06:41:56
-->

# Longest substring without repeating characters program in Go (Golang)

> 来源：[https://golangbyexample.com/longest-substring-without-repeating-characters-golang/](https://golangbyexample.com/longest-substring-without-repeating-characters-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

A string is given and we have to find the longest substring in it without repeating characters. For eg if the string is

```
abbabcda
```

Then the answer would be **“abcd”** and the length should be 4.

We use a hash and three variables

*   Hash keeps track of the last index position of any character

*   longestSubstringLength – this holds the longest substring length without repeating character seen so far

*   currentSubstringLength – this holds the current substring length without repeating character

*   start – this indicates the start of the current substring without repeating character

We iterate over the string and check this hash for the current character. We simply increment the **currentSubstringLength** in the below two conditions

*   If the entry is not present for the current character then the current character has not been seen before.

*   If the entry is present and the current character has been seen before but it is not part of the current length.

Otherwise

*   We reset the **start** position and **currentSubstringLength** to include the current character in the current length. Before resetting we check if **currentSubstringLength** is greater than **longestSubstringLength**. If yes then we set **longestSubstringLength** to **currentSubstringLength**.

Let’s see a program for the same

## **Program**

```
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

**Output**

```
4
```*