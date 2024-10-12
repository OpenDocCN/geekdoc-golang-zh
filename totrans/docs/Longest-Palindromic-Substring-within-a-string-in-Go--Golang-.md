<!--yml
category: 未分类
date: 2024-10-13 06:42:04
-->

# Longest Palindromic Substring within a string in Go (Golang)

> 来源：[https://golangbyexample.com/longest-palindromic-substring-go/](https://golangbyexample.com/longest-palindromic-substring-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the largest palindromic substring within a string. For eg let’s say the input string is

```
cabae
```

The output should be **aba** which is the largest palindromic substring.

We will use Dynamic Programming to solve this problem. We will use two matrices. Each matrix will be of the size **len*len** where **len** is the size of the input string

*   The first matrix will store whether the substring between index **“i”** and **“j”** is palindrome or not.

*   The second matrix will store the longest palindromic substring between **“i”** and **“j”.**

Below is the optimal substructure

*   If **string[i] == string[j]** and substring in **i+1** and **j-1** is a palidrome, then

```
len(i,j) = 2 + len(i+1, j-1)
```

*   If **string[i] != string[j]** then

```
len(i,j) = max(len(i+1,j), len(i,j-1)
```

## **Program**

Below is the program for the same

```
package main

import (
	"fmt"
	"math"
	"unicode/utf8"
)

func main() {
	output := longestPalindrome("cabae")
	fmt.Println(output)
}

func longestPalindrome(s string) string {
	stringLength := utf8.RuneCountInString(s)
	isPalindromeMatrix := make([][]int, stringLength)
	for i := range isPalindromeMatrix {
		isPalindromeMatrix[i] = make([]int, stringLength)
	}

	for i, outer := range isPalindromeMatrix {
		for j := range outer {
			if i == j {
				isPalindromeMatrix[i][j] = 1
			}

		}
	}

	palindromeLengthMatrix := make([][]int, stringLength)
	for i := range palindromeLengthMatrix {
		palindromeLengthMatrix[i] = make([]int, stringLength)
	}

	for i, outer := range palindromeLengthMatrix {
		for j := range outer {
			if i == j {
				palindromeLengthMatrix[i][j] = 1
			}

		}
	}

	for len := 2; len <= stringLength; len++ {
		for i := 0; i <= stringLength-len; i++ {
			j := i + len - 1

			if s[i] == s[j] {
				if len == 2 {
					isPalindromeMatrix[i][j] = 1
					palindromeLengthMatrix[i][j] = 2
				} else {
					if isPalindromeMatrix[i+1][j-1] == 1 {
						isPalindromeMatrix[i][j] = 1
						palindromeLengthMatrix[i][j] = 2 + palindromeLengthMatrix[i+1][j-1]
					} else {
						isPalindromeMatrix[i][j] = -1
						palindromeLengthMatrix[i][j] = int(math.Max(float64(palindromeLengthMatrix[i+1][j]), float64(palindromeLengthMatrix[i][j-1])))
					}

				}

			} else {
				isPalindromeMatrix[i][j] = -1
			}

		}
	}

	max_row_index := 0
	max_column_index := 0
	max := 0

	for i, outer := range palindromeLengthMatrix {
		for j := range outer {
			if palindromeLengthMatrix[i][j] > max && isPalindromeMatrix[i][j] == 1 {
				max = palindromeLengthMatrix[i][j]
				max_row_index = i
				max_column_index = j
			}
		}
	}

	return s[max_row_index : max_column_index+1]
}
```

**Output**

```
aba
```*