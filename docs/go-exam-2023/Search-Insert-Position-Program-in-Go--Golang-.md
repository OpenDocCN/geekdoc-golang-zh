<!--yml

类别：未分类

日期：2024-10-13 06:48:45

-->

# 在Go (Golang)中搜索插入位置程序

> 来源：[https://golangbyexample.com/search-insert-position-golang/](https://golangbyexample.com/search-insert-position-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

给定一个有序的输入数组，包含不同的整数和一个目标值。目标是找到该目标值在该数组中的插入位置。有两种情况

+   如果目标值在给定数组中存在，则返回该索引。

+   如果目标值在给定数组中不存在，返回将插入的位置

示例

```
Input: [1,2,3]
Target Value: 4
Output: 3

Input: [1,2,3]
Target Value: 3
Output: 2
```

## **程序**

这是相同程序的内容。

```
package main

import "fmt"

func searchInsert(nums []int, target int) int {
	lenNums := len(nums)

	index := -1

	if target <= nums[0] {
		return 0
	}

	if target > nums[lenNums-1] {
		return lenNums
	}

	for i := 0; i < lenNums; i++ {
		if target <= nums[i] {
			index = i
			break
		}
	}

	return index

}

func main() {
	pos := searchInsert([]int{1, 2, 3}, 4)
	fmt.Println(pos)

	pos = searchInsert([]int{1, 2, 3}, 3)
	fmt.Println(pos)
}
```

**输出**

```
3
2
```

**注意：** 请查看我们的Golang高级教程。本系列的教程详尽全面，我们努力覆盖所有概念并附有示例。本教程适合那些希望获得专业知识并深入理解golang的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对理解如何在Golang中实现所有设计模式感兴趣。如果是，那么这篇文章适合你 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
