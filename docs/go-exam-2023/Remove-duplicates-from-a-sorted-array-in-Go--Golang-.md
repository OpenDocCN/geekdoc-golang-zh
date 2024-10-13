<!--yml

分类：未分类

日期：2024-10-13 06:45:46

-->

# 在 Go（Golang）中从已排序的数组中移除重复项

> 来源：[`golangbyexample.com/remove-duplicates-sorted-array-golang/`](https://golangbyexample.com/remove-duplicates-sorted-array-golang/)

目录

+   概述

+   程序

## **概述**

目标是从一个已排序的数组中移除重复项。

**示例**

```go
Input: [1, 1, 1, 2]
Output: [1, 2]

Input: [1, 2, 3, 3]
Output: [1, 2, 3]
```

## **程序**

以下是相同的程序

```go
package main

import "fmt"

func main() {
	input := []int{1, 1, 1, 2}
	output := removeDuplicates(input)
	fmt.Println(output)

	input = []int{1, 2, 3, 3}
	output = removeDuplicates(input)
	fmt.Println(output)
}

func removeDuplicates(nums []int) []int {
	lenArray := len(nums)

	k := 0
	for i := 0; i < lenArray; {
		nums[k] = nums[i]
		k++
		for i+1 < lenArray && nums[i] == nums[i+1] {
			i++
		}
		i++
	}

	return nums[0:k]
}
```

**输出**

```go
[1 2]
[1 2 3]
```

**注意：**查看我们的 Golang 高级教程。本系列教程内容详尽，我们尝试覆盖所有概念并附有示例。本教程适合那些希望获得专业知识和扎实理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
