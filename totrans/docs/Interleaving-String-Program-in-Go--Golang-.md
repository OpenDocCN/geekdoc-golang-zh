<!--yml
category: 未分类
date: 2024-10-13 06:44:48
-->

# Interleaving String Program in Go (Golang)

> 来源：[https://golangbyexample.com/interleaving-string-program-golang/](https://golangbyexample.com/interleaving-string-program-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Recursive Solution](#Recursive_Solution "Recursive Solution")
*   [Dynamic Programming Solution](#Dynamic_Programming_Solution "Dynamic Programming Solution")*  *## **Overview**

Three strings are given **s1**, **s2**, **s3**. Find if string **s3** is interleaving of string.

**s3** will be an interleaving of string **s1** and **s2** if the below condition is satisfied

*   s3 contains all characters of **s1** and **s2** and the order of all characters in individual strings is preserved.

Example

```
s1: aabcc
s2: dbbca
s3: aadbbcbcac

Output: true
```

## **Recursive Solution**

Below is the recursive solution for the same

```
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

**Output**

```
true
true
```

If you will notice the above program many subproblems are computed again and again hence the complexity of the above solution is exponential. Hence we can also use Dynamic Programming here to reduce the overall time complexity.

Here is the program for the same

## **Dynamic Programming Solution**

```
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

**Output**

```
true
true
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*