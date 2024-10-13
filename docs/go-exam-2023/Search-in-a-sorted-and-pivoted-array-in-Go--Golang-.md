<!--yml

类别：未分类

日期：2024-10-13 06:43:50

-->

# 在 Go (Golang) 中对排序和旋转数组进行搜索

> 来源：[https://golangbyexample.com/search-sorted-pivoted-array/](https://golangbyexample.com/search-sorted-pivoted-array/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

我们有一个已排序但在某个索引处旋转的输入数组。例如，考虑以下数组

```
[1, 3, 5, 7, 9]
```

它已在索引 3 处旋转和旋转

```
[5, 7, 9, 1, 3]
```

给定一个目标元素，目标是在该排序和旋转数组中以 **O(logn)** 的时间找到目标元素的索引。如果目标元素不存在于给定数组中，则应返回 -1

例如

```
Input: [5, 7, 9, 1, 3]
Target Element: 7
Output: 2

Input: [5, 7, 9, 1, 3]
Target Element: 8
Output: -1
```

以下是策略

+   使用一些修改的二分查找法在给定的输入数组中找到旋转点索引

+   如果目标元素小于数组的起始元素，则从旋转点到数组末尾进行二分查找

+   如果目标元素大于数组的起始元素，则从起始位置到旋转点 - 1 索引进行二分查找

以下是找到旋转点索引的策略

+   进行二分查找。对于每个中间元素，检查 **mid** 或 **mid+1** 是否为旋转点

+   如果 **mid** 的值小于输入数组起始值，则在 **mid** 的左侧进行搜索

+   如果 mid 的值大于输入数组的起始值，则在 **mid** 的右侧进行搜索

## **程序**

以下是相应的程序。

```
package main

import "fmt"

func main() {
	output := search([]int{4, 5, 6, 7, 0, 1, 2}, 0)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 1)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 2)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 4)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 5)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 6)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 7)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 3)
	fmt.Println(output)

	output = search([]int{1, 2}, 3)
	fmt.Println(output)
}

func search(nums []int, target int) int {
	pivot := findPivot(nums)

	if pivot == -1 {
		return binarySearch(nums, 0, len(nums)-1, target)
	}

	if target < nums[0] {
		return binarySearch(nums, pivot, len(nums)-1, target)
	}

	return binarySearch(nums, 0, pivot-1, target)
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

```
4
5
6
0
1
2
3
-1
-1
```

**注意：** 请查看我们的 Golang 高级教程。本系列的教程详细且尽力覆盖所有概念和示例。本教程适合那些希望获得专业知识和对 Golang 有深入理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果您有兴趣了解如何在 Golang 中实现所有设计模式。如果是，那么这篇文章适合您 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
