<!--yml

分类：未分类

日期：2024-10-13 06:50:14

-->

# Go（Golang）中在已排序数组中进行二分查找的程序

> 来源：[https://golangbyexample.com/binary-search-sorted-array-golang/](https://golangbyexample.com/binary-search-sorted-array-golang/)

目录

**   [概述](#Overview "概述")

+   [程序](#Program "程序")*  *## **概述**

思路是在输入数组中对给定的目标元素进行二分查找。如果目标元素存在，则输出其索引。如果输出元素不存在，则输出 -1。

预期时间复杂度为 O(logn)

**示例 1**

```go
Input: [1, 4, 5, 6]
Target Element: 4
Output: 1

Target element 4 is present at index 1
```

**示例 2**

```go
Input: [1, 2, 3]
Target Element: 4
Output: -1

Target element 4 is present at index 1
```

## **程序**

```go
package main

import "fmt"

func search(nums []int, target int) int {
	return binarySearch(nums, 0, len(nums)-1, target)
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

func main() {
	index := search([]int{1, 4, 5, 6}, 4)
	fmt.Println(index)

	index = search([]int{1, 2, 3, 6}, 4)
	fmt.Println(index)
}
```

**输出**

```go
1
-1
```

**注意：** 请查看我们的 Golang 高级教程。该系列教程详尽，我们努力涵盖所有概念并提供示例。本教程适合那些希望获得 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对如何在 Golang 中实现所有设计模式感兴趣，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

此外，请查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*
