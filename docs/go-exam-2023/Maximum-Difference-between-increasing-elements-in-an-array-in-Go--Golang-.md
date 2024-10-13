<!--yml

类别：未分类

日期：2024-10-13 06:49:48

-->

# 在 Go (Golang)中数组中递增元素之间的最大差异

> 来源：[`golangbyexample.com/maximum-difference-increasing-elements-in-an-array-golang/`](https://golangbyexample.com/maximum-difference-increasing-elements-in-an-array-golang/)

目录

+   概述

+   程序

## **概述**

给定一个数组。目标是找到两个索引 i 和 j 之间的最大差异，使得

+   j > i

+   arr[j] > arr[i]

如果不存在这样的索引，则返回-1

示例 1

```go
Input: intervals = [8, 2, 6, 5]
Output: 4
Explanation: 6-2 = 4
```

示例 2

```go
Input: intervals = [8, 3, 2, 1]
Output: -1
Explanation: Condition is not satified
```

## **程序**

这是相应的程序。

```go
package main

import "fmt"

func maximumDifference(nums []int) int {
	lenNums := len(nums)
	if lenNums == 0 || lenNums == 1 {
		return -1
	}
	minElement := nums[0]

	maxDifference := -1

	for i := 1; i < lenNums; i++ {
		diff := nums[i] - minElement
		if diff > maxDifference && diff != 0 {
			maxDifference = diff
		}

		if nums[i] < minElement {
			minElement = nums[i]
		}
	}

	return maxDifference
}

func main() {
	output := maximumDifference([]int{8, 2, 6, 5})
	fmt.Println(output)

	output = maximumDifference([]int{8, 3, 2, 1})
	fmt.Println(output)

}
```

**输出**

```go
4
-1
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，尽力涵盖所有概念并附带示例。该教程适合希望获得专业知识和扎实理解的 Golang 学习者 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


