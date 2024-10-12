<!--yml
category: 未分类
date: 2024-10-13 06:44:43
-->

# Edit Distance between two strings in Go (Golang)

> 来源：[https://golangbyexample.com/edit-distance-two-strings-golang/](https://golangbyexample.com/edit-distance-two-strings-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Recursive Solution](#Recursive_Solution "Recursive Solution")
*   [Dynamic Programming Solution](#Dynamic_Programming_Solution "Dynamic Programming Solution")*  *## **Overview**

Given two strings find the minimum number of operations to convert one string into another. A string can be converted into another string by performing below three operations

*   Insert

*   Remove

*   Replace

All the operations are of equal cost.  Let’s see some examples

**Example 1:**

```
First String: abc
Second String: abcd
Output: 1
```

We can insert a **‘d’** in the first string

**Example 2:**

```
First String: abc
Second String: ab
Output: 1
```

We can remove the **‘c’** from the first string

**Example 3:**

```
First String: abc
Second String: abd
Output: 1
```

We can replace the **‘c’** with **‘d’** in the first string

**Example 4:**

```
First String: abce
Second String: abd
Output: 2
```

*   We can remove **‘e’** from the first string

*   We can replace **‘c’** with **‘d’** in the first string

Some base cases. Assume the first string is of length **m** and the second string is of length **n**

*   If both the first and second string is of zero length then the output is 0

*   If only the first string is empty then the output is the length of the second string

*   If only the second string is empty then the output is the length of the first string

Otherwise

If the last characters of the first and second string match then the output is the min edit operations with the first string of length **m-1** and the second string of length **n-1**. It means recurse for (m-1, n-1)If the last characters do not match then we can either do an insert, remove or replace option in the first string.

*   Replace – recurse for (m-1, n-1)

*   Remove – recurse for (m,-1 n)

*   Insert – recurse for (m, n-1)

## **Recursive Solution**

Below is the recursive solution for the same problem

```
package main

import (
	"fmt"
	"math"
)

func main() {
	output := minDistance("abc", "abcd")
	fmt.Println(output)

	output = minDistance("abc", "ab")
	fmt.Println(output)

	output = minDistance("abc", "abd")
	fmt.Println(output)

	output = minDistance("abce", "abd")
	fmt.Println(output)
}

func minDistance(word1 string, word2 string) int {
	word1Rune := []rune(word1)
	word2Rune := []rune(word2)
	lenWord1 := len(word1Rune)
	lenWord2 := len(word2Rune)

	return minDistanceUtil(word1Rune, word2Rune, lenWord1, lenWord2)
}

func minDistanceUtil(word1 []rune, word2 []rune, lenWord1, lenWord2 int) int {
	if lenWord1 == 0 && lenWord2 == 0 {
		return 0
	}

	if lenWord1 == 0 {
		return lenWord2
	}

	if lenWord2 == 0 {
		return lenWord1
	}

	if word1[lenWord1-1] == word2[lenWord2-1] {
		return minDistanceUtil(word1, word2, lenWord1-1, lenWord2-1)
	} else {
		x := minDistanceUtil(word1, word2, lenWord1-1, lenWord2-1)
		y := minDistanceUtil(word1, word2, lenWord1, lenWord2-1)
		z := minDistanceUtil(word1, word2, lenWord1-1, lenWord2)
		return 1 + minOfThree(x, y, z)
	}
}

func minOfThree(x, y, z int) int {
	output := int(math.Min(float64(x), math.Min(float64(y), float64(z))))
	return output
}
```

**Output**

```
1
1
1
2
```

If you will notice the above program many subproblems are computed again and again hence the complexity of the above solution is exponential. Hence we can also use Dynamic Programming here to reduce the overall time complexity.

Here is the program for the same

## **Dynamic Programming Solution**

```
package main

import (
	"fmt"
	"math"
)

func main() {
	output := minDistance("abc", "abcd")
	fmt.Println(output)

	output = minDistance("abc", "ab")
	fmt.Println(output)

	output = minDistance("abc", "abd")
	fmt.Println(output)

	output = minDistance("abce", "abd")
	fmt.Println(output)
}

func minDistance(word1 string, word2 string) int {
	word1Rune := []rune(word1)
	word2Rune := []rune(word2)
	lenWord1 := len(word1Rune)
	lenWord2 := len(word2Rune)

	editDistanceMatrix := make([][]int, lenWord1+1)

	for i := range editDistanceMatrix {
		editDistanceMatrix[i] = make([]int, lenWord2+1)
	}

	for i := 1; i <= lenWord2; i++ {
		editDistanceMatrix[0][i] = i
	}

	for i := 1; i <= lenWord1; i++ {
		editDistanceMatrix[i][0] = i
	}
	for i := 1; i <= lenWord1; i++ {
		for j := 1; j <= lenWord2; j++ {

			if word1Rune[i-1] == word2Rune[j-1] {
				editDistanceMatrix[i][j] = editDistanceMatrix[i-1][j-1]
			} else {
				editDistanceMatrix[i][j] = 1 + minOfThree(editDistanceMatrix[i-1][j], editDistanceMatrix[i][j-1], editDistanceMatrix[i-1][j-1])
			}
		}
	}
	return editDistanceMatrix[lenWord1][lenWord2]
}

func minOfThree(x, y, z int) int {
	output := int(math.Min(float64(x), math.Min(float64(y), float64(z))))
	return output
}
```

**Output**

```
1
1
1
2
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)*