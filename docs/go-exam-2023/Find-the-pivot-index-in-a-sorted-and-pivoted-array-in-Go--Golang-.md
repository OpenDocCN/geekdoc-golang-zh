<!--yml

分类：未分类

日期：2024-10-13 06:43:45

-->

# 在 Go (Golang) 中找到排序和基准化数组中的基准索引。

> 来源：[`golangbyexample.com/pivot-index-pivoted-array-go/`](https://golangbyexample.com/pivot-index-pivoted-array-go/)

目录

+   概述

## **概述**

我们有一个已排序的输入数组，但在某个索引处被基准化。例如，考虑下面的数组。

```go
[1, 3, 5, 7, 9]
```

它在索引 3 处被旋转和基准化。

```go
[5, 7, 9, 1, 3]
```

目标是找到基准索引。因此，上述数组的答案将是 3。如果数组没有被基准化，则应返回-1。例如，对于下面的输入数组，它应该返回-1。

```go
[0, 1, 2, 3, 4]
```

其他示例

```go
Pivoted Array: [7, 9, 1, 3, 5]
Pivot Index: 2

Pivoted Array: [9, 1, 3, 5, 9]
Pivot Index: 2

Pivoted Array: [1, 3, 5, 7, 9]
Pivot Index: -1
```

找到基准索引的预期时间复杂度为**O(logn)**。因此，我们必须进行一些修改的二分搜索来找到基准索引。

这将是策略。

+   进行二分搜索。对于每个中间元素，检查**mid**或**mid+1**是否为基准。

+   如果**mid**的值小于输入数组的起始值，则在**mid**的左侧进行搜索。

+   如果 mid 的值大于输入数组起始值，则在**mid**的右侧进行搜索。

下面是相应的程序。

```go
package main

import "fmt"

func main() {
	pivot := findPivot([]int{0, 1, 2, 3, 4, 5})
	fmt.Println(pivot)

	pivot = findPivot([]int{1, 2, 3, 4, 5, 0})
	fmt.Println(pivot)

	pivot = findPivot([]int{2, 3, 4, 5, 0, 1})
	fmt.Println(pivot)

	pivot = findPivot([]int{3, 4, 5, 0, 1, 2})
	fmt.Println(pivot)

	pivot = findPivot([]int{4, 5, 0, 1, 2, 3})
	fmt.Println(pivot)
	pivot = findPivot([]int{5, 0, 1, 2, 3, 4})
	fmt.Println(pivot)
}

func findPivot(nums []int) int {
	return findPivotUtil(nums, 0, len(nums)-1)
}

func findPivotUtil(nums []int, start, end int) int {
	if start > end {
		return -1
	}

	mid := (start + end) / 2

	if mid+1 <= end && nums[mid] > nums[mid+1] {
		return mid + 1
	}

	if mid-1 >= start && nums[mid] < nums[mid-1] {
		return mid
	}

	if nums[mid] < nums[start] {
		return findPivotUtil(nums, start, mid-1)
	}

	return findPivotUtil(nums, mid+1, end)

}

func binarySearch(nums []int, start, end, target int) int {
	if start > end {
		return -1
	}

	mid := (start + end) / 2

	if nums[mid] == target {
		return mid
	}

	if target < nums[mid] {
		return binarySearch(nums, start, mid-1, target)
	} else {
		return binarySearch(nums, mid+1, end, target)
	}

}
```

**输出**

```go
-1
5
4
3
2
1
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力覆盖所有概念并附有示例。这个教程适合那些希望获得专业知识和对 Golang 有深入理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


