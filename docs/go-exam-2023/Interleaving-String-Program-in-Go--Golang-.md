<!--yml

分类：未分类

日期：2024-10-13 06:44:48

-->

# Go 语言中的交错字符串程序

> 来源：[`golangbyexample.com/interleaving-string-program-golang/`](https://golangbyexample.com/interleaving-string-program-golang/)

目录

**   概述

+   递归解决方案

+   动态规划解决方案*  *## **概述**

给定三个字符串**s1**、**s2**、**s3**。判断字符串**s3**是否为字符串的交错。

如果满足以下条件，**s3**将是字符串**s1**和**s2**的交错。

+   s3 包含**s1**和**s2**的所有字符，并且每个字符串中的所有字符顺序保持不变。

示例

```go
s1: aabcc
s2: dbbca
s3: aadbbcbcac

Output: true
```

## **递归解决方案**

以下是相同的递归解决方案

```go
package main

import "fmt"

func main() {
	output := isInterleave("aabcc", "dbbca", "aadbbcbcac")
	fmt.Println(output)

	output = isInterleave("", "", "")
	fmt.Println(output)
}
func isInterleave(s1 string, s2 string, s3 string) bool {
	s1Rune := []rune(s1)
	s2Rune := []rune(s2)
	s3Rune := []rune(s3)

	lenS1 := len(s1Rune)
	lenS2 := len(s2Rune)
	lenS3 := len(s3Rune)

	if lenS1+lenS2 != lenS3 {
		return false
	}

	return isInterleaveUtil(s1Rune, s2Rune, s3Rune, 0, 0, 0, lenS1, lenS2, lenS3)
}

func isInterleaveUtil(s1, s2, s3 []rune, x, y, z, lenS1, lenS2, lenS3 int) bool {
	if x == lenS1 && y == lenS2 && z == lenS3 {
		return true
	}

	if x < lenS1 && z < lenS3 && s1[x] == s3[z] {
		match := isInterleaveUtil(s1, s2, s3, x+1, y, z+1, lenS1, lenS2, lenS3)
		if match {
			return true
		}
	}

	if y < lenS2 && z < lenS3 && s2[y] == s3[z] {
		return isInterleaveUtil(s1, s2, s3, x, y+1, z+1, lenS1, lenS2, lenS3)

	}
	return false
}
```

**输出**

```go
true
true
```

如果你注意到上述程序，许多子问题被反复计算，因此上述解决方案的复杂性是指数级的。因此，我们也可以在这里使用动态规划来减少整体时间复杂度。

这是相同程序的代码

## **动态规划解决方案**

```go
package main

import "fmt"

func main() {
	output := isInterleave("aabcc", "dbbca", "aadbbcbcac")
	fmt.Println(output)

	output = isInterleave("", "", "")
	fmt.Println(output)
}
func isInterleave(s1 string, s2 string, s3 string) bool {
	s1Rune := []rune(s1)
	s2Rune := []rune(s2)
	s3Rune := []rune(s3)

	lenS1 := len(s1Rune)
	lenS2 := len(s2Rune)
	lenS3 := len(s3Rune)

	if lenS1+lenS2 != lenS3 {
		return false
	}

	interleavingMatrix := make([][]bool, lenS1+1)

	for i := range interleavingMatrix {
		interleavingMatrix[i] = make([]bool, lenS2+1)
	}

	i := 1
	k := 1

	interleavingMatrix[0][0] = true

	for i <= lenS1 && k <= lenS3 {
		if s1Rune[i-1] == s3Rune[k-1] {
			interleavingMatrix[i][0] = true
			i++
			k++
		} else {
			break
		}
	}

	i = 1
	k = 1

	for i <= lenS2 && k <= lenS3 {
		if s2Rune[i-1] == s3Rune[k-1] {
			interleavingMatrix[0][i] = true
			i++
			k++
		} else {
			break
		}
	}

	for i := 1; i <= lenS1; i++ {
		for j := 1; j <= lenS2; j++ {

			if s1Rune[i-1] == s3Rune[i+j-1] {
				interleavingMatrix[i][j] = interleavingMatrix[i-1][j]
			}

			if s2Rune[j-1] == s3Rune[i+j-1] && !interleavingMatrix[i][j] {
				interleavingMatrix[i][j] = interleavingMatrix[i][j-1]
			}

		}
	}

	return interleavingMatrix[lenS1][lenS2]
}
```

**输出**

```go
true
true
```

**注意：**查看我们的 Golang 高级教程。本系列的教程详细且我们尽力涵盖所有概念及示例。本教程适合希望获得 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
