<!--yml

分类：未分类

日期：2024-10-13 06:42:04

-->

# Go语言中的字符串最长回文子串

> 来源：[https://golangbyexample.com/longest-palindromic-substring-go/](https://golangbyexample.com/longest-palindromic-substring-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是在字符串中找到最大的回文子串。例如，假设输入字符串为

```go
cabae
```

输出应该是**aba**，它是最大的回文子串。

我们将使用动态编程来解决这个问题。我们将使用两个矩阵。每个矩阵的大小为**len*len**，其中**len**是输入字符串的大小。

+   第一个矩阵将存储索引**“i”**和**“j”**之间的子串是否为回文。

+   第二个矩阵将存储**“i”**和**“j”**之间的最长回文子串。

以下是最优子结构。

+   如果**string[i] == string[j]**且**i+1**到**j-1**的子串是回文，则

```go
len(i,j) = 2 + len(i+1, j-1)
```

+   如果**string[i] != string[j]**，那么

```go
len(i,j) = max(len(i+1,j), len(i,j-1)
```

## **程序**

以下是相应的程序。

```go
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

**输出**

```go
aba
```*
