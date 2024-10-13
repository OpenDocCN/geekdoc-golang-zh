<!--yml

类别：未分类

日期：2024-10-13 06:49:53

-->

# 在 Go (Golang) 中，具有相等数量的 0 和 1 的连续子数组的最大长度

> 来源：[`golangbyexample.com/max-length-array-zero-one-golang/`](https://golangbyexample.com/max-length-array-zero-one-golang/)

目录

**   概述

+   程序*  *## **概述**

给定一个仅包含 0 和 1 的数组。目标是找到一个具有相等数量的 0 和 1 的最大长度子数组。让我们通过示例来理解它。

示例 1

```go
Input: intervals = [0, 1]
Output: 2
```

示例 2

```go
Input: intervals = [0, 1, 1, 0, 1, 1]
Output: 4
```

以下是我们可以采取的方法

+   我们将创建一个数组 **leftSum**，其中 leftSum[i] 表示从索引 0 到 i 的数字之和。将 0 视为 -1，将 1 视为 1。

+   现在有两种情况。子数组从索引 0 开始，或者子数组从其他索引开始。

+   从左到右扫描 leftSum 数组。如果 **leftSum** 中任何索引的值为零，则表示 subarray[0,i] 中包含相同数量的 0 和 1。这将给出如果子数组从索引 0 开始的答案。

+   如果子数组不从零开始。那么再次扫描 leftSum 数组，找到索引 **i** 和 **j**，使得 **leftSum[i] == leftSum[j]**。为了弄清楚这一点，我们将使用一个映射。如果 j-i 的长度大于最大长度，则更新最大长度。

+   最后返回最大长度

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func findMaxLength(nums []int) int {
	lenNums := len(nums)

	if lenNums == 0 {
		return 0
	}

	currentSum := 0

	sumLeft := make([]int, lenNums)

	for i := 0; i < lenNums; i++ {
		if nums[i] == 0 {
			currentSum = currentSum - 1
		} else {
			currentSum = currentSum + 1
		}
		sumLeft[i] = currentSum
	}

	maxLength := 0

	max := 0
	min := 0

	for i := 0; i < lenNums; i++ {
		if sumLeft[i] == 0 {
			maxLength = i + 1
		}
		if sumLeft[i] > max {
			max = sumLeft[i]
		}

		if sumLeft[i] < min {
			min = sumLeft[i]
		}
	}

	numMap := make(map[int]int, max-min+1)

	for i := 0; i < lenNums; i++ {
		index, ok := numMap[sumLeft[i]]

		if ok {
			currentLength := i - index
			if currentLength > maxLength {
				maxLength = currentLength
			}
		} else {
			numMap[sumLeft[i]] = i
		}
	}

	return maxLength

}
func main() {
	output := findMaxLength([]int{0, 1})
	fmt.Println(output)

	output = findMaxLength([]int{0, 1, 1, 0, 1, 1})
	fmt.Println(output)
}
```

**输出**

```go
2
4
```

**注意：** 查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试用示例覆盖所有概念。本教程适合那些希望获得 Golang 专业知识和深入理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是这样，那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*
