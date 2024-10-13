<!--yml

类别：未分类

日期：2024-10-13 06:44:43

-->

# 在Go语言中计算两个字符串之间的编辑距离（Golang）。

> 来源：[https://golangbyexample.com/edit-distance-two-strings-golang/](https://golangbyexample.com/edit-distance-two-strings-golang/)

目录

**   [概述](#Overview "Overview")**

+   [递归解决方案](#Recursive_Solution "递归解决方案")

+   [动态规划解决方案](#Dynamic_Programming_Solution "动态规划解决方案")*  *## **概述**

给定两个字符串，找出将一个字符串转换为另一个字符串所需的最小操作次数。可以通过执行以下三种操作将一个字符串转换为另一个字符串：

+   插入

+   移除

+   替换

所有操作的成本相等。让我们来看一些示例。

**示例 1：**

```
First String: abc
Second String: abcd
Output: 1
```

我们可以在第一个字符串中插入**‘d’**。

**示例 2：**

```
First String: abc
Second String: ab
Output: 1
```

我们可以从第一个字符串中移除**‘c’**。

**示例 3：**

```
First String: abc
Second String: abd
Output: 1
```

我们可以在第一个字符串中将**‘c’**替换为**‘d’**。

**示例 4：**

```
First String: abce
Second String: abd
Output: 2
```

+   我们可以从第一个字符串中移除**‘e’**。

+   我们可以在第一个字符串中将**‘c’**替换为**‘d’**。

一些基本情况。假设第一个字符串的长度为**m**，第二个字符串的长度为**n**。

+   如果第一个字符串和第二个字符串的长度均为零，则输出为0。

+   如果只有第一个字符串为空，则输出为第二个字符串的长度。

+   如果只有第二个字符串为空，则输出为第一个字符串的长度。

否则

如果第一个字符串和第二个字符串的最后一个字符匹配，则输出的是对第一个字符串长度为**m-1**和第二个字符串长度为**n-1**的最小编辑操作。这意味着递归计算（m-1, n-1）。如果最后一个字符不匹配，则我们可以在第一个字符串中进行插入、删除或替换操作。

+   替换 – 递归计算（m-1, n-1）

+   移除 – 递归计算（m,-1 n）

+   插入 – 递归计算（m, n-1）

## **递归解决方案**

下面是相同问题的递归解决方案。

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

**输出**

```
1
1
1
2
```

如果你注意到上面的程序，许多子问题被反复计算，因此上述解决方案的复杂度是指数级的。因此，我们也可以在这里使用动态规划来降低整体时间复杂度。

这是相同的程序。

## **动态规划解决方案**

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

**输出**

```
1
1
1
2
```

**注意：** 请查看我们的Go语言高级教程。本系列的教程内容详尽，我们试图通过示例覆盖所有概念。本教程适合那些希望获得专业知识和对Go语言有深入理解的人 - [Go语言高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在Golang中实现所有设计模式。如果是的话，这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)*
