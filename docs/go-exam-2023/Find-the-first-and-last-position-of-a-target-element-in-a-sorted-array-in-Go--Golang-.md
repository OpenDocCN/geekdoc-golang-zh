<!--yml

类别：未分类

日期：2024-10-13 06:44:00

-->

# 在Go（Golang）中查找有序数组中目标元素的第一个和最后一个位置

> 来源：[https://golangbyexample.com/first-last-position-target-sorted-array-golang/](https://golangbyexample.com/first-last-position-target-sorted-array-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是找到有序数组中元素的第一个和最后一个位置。

例如

```
Input: [1, 2, 2, 5]
Target: 2
Output: [1, 2]

Input: [1, 2, 5]
Target: 2
Output: [1, 1]

Input: []
Target: 1
Output: [-1,-1]
```

策略是

+   在数组中进行二分查找，找到左侧索引。

+   然后在数组中再次进行二分查找，找到正确的索引。

## **程序**

以下是相应的程序

```
package main

import "fmt"

func main() {
	output := searchRange([]int{1, 2, 2, 5}, 2)
	fmt.Println(output)

	output = searchRange([]int{1, 2, 5}, 2)
	fmt.Println(output)

	output = searchRange([]int{}, 1)
	fmt.Println(output)
}

func searchRange(nums []int, target int) []int {

	output := make([]int, 2)
	output[0] = findLeftPivot(nums, 0, len(nums)-1, target, len(nums))

	output[1] = findRightPivot(nums, 0, len(nums)-1, target, len(nums))

	return output
}

func findLeftPivot(nums []int, start, end, target, len int) int {
	if start > end {
		return -1
	}

	if start == end && nums[start] == target {
		return start
	}

	mid := (start + end) / 2

	if (mid == 0 || nums[mid-1] < nums[mid]) && nums[mid] == target {
		return mid
	}

	if target <= nums[mid] {
		return findLeftPivot(nums, start, mid-1, target, len)
	}

	return findLeftPivot(nums, mid+1, end, target, len)

}

func findRightPivot(nums []int, start, end, target, len int) int {
	if start > end {
		return -1
	}

	if start == end && nums[start] == target {
		return start

	}

	mid := (start + end) / 2

	if mid+1 <= end && nums[mid] == target && nums[mid] < nums[mid+1] {
		return mid
	}

	if (mid == len-1 || nums[mid] < nums[mid+1]) && nums[mid] == target {
		return mid - 1
	}

	if target >= nums[mid] {
		return findRightPivot(nums, mid+1, end, target, len)
	}

	return findRightPivot(nums, start, mid-1, target, len)

}
```

**输出**

```
[1 2]
[1 1]
[-1 -1]
```

**注意：** 查看我们的Golang高级教程。本系列的教程内容详尽，我们尝试涵盖所有概念并附上示例。本教程适合希望获得专业知识和扎实理解Golang的人 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对理解所有设计模式在Golang中的实现感兴趣。如果是的话，这篇文章适合你 – [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
