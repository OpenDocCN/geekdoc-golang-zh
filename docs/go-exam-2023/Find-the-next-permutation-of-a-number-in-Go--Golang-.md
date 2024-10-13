<!--yml

类别：未分类

日期：2024-10-13 06:42:54

-->

# 在Go中找到一个数字的下一个排列

> 来源：[https://golangbyexample.com/next-permuation-number-go/](https://golangbyexample.com/next-permuation-number-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是根据字典序排序找到给定数字的下一个排列。如果下一个排列不可行，则返回相同的数字

例如

```
Input: [1,2,3]
Output: [1,3,2]

Input: [2,1]
Output: [2,1]

Input: [1, 3, 5, 4, 1]
Output: [1, 4, 1, 3, 5]

Input: [1, 3, 2]
Output: [2, 1, 3]
```

下面将是策略

+   从左侧开始，找到第一个小于其右侧数字的数字。假设该数字在索引**“first”**处找到

+   然后找到数组右侧在索引**first**之后大于该数字的最小值。然后用索引**first**的数字替换该数字

+   对索引**first**之后的数组右部分进行排序

## **程序**

下面是相同的程序

```
package main

import (
	"fmt"
	"sort"
)

func main() {
	nextPermutation([]int{1, 2, 3})

	nextPermutation([]int{2, 1})

	nextPermutation([]int{1, 3, 5, 4, 1})

	nextPermutation([]int{1, 3, 2})
}

func nextPermutation(nums []int) {

	numsLen := len(nums)
	first := -1
	second := -1

	for i, j := numsLen-2, numsLen-1; i >= 0; {
		if nums[i] < nums[j] {
			first = i
			second = j
			break
		} else {
			i--
			j--
		}
	}

	if !(first == -1) {
		smallestGreaterIndex := second
		for i := second + 1; i < numsLen; i++ {
			if nums[i] > nums[first] && nums[i] < nums[smallestGreaterIndex] {
				smallestGreaterIndex = i
			}
		}
		nums[first], nums[smallestGreaterIndex] = nums[smallestGreaterIndex], nums[first]

		sort.Slice(nums[second:numsLen], func(i, j int) bool {
			return nums[second+i] < nums[second+j]
		})
	}

	fmt.Println(nums)

}
```

**输出**

```
[1 3 2]
[2 1]
[1 4 1 3 5]
[2 1 3]
```

**注意：** 查看我们的Golang高级教程。本系列的教程内容详尽，我们努力涵盖所有概念并提供示例。此教程适合希望获得专业知识和对Golang有扎实理解的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在Golang中实现所有设计模式。如果是的话，那么这篇文章适合你 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*
