<!--yml

类别: 未分类

日期: 2024-10-13 06:49:25

-->

# 字符串或数组在Go（Golang）中的不同或唯一排列

> 来源：[https://golangbyexample.com/unique-permutations-golang/](https://golangbyexample.com/unique-permutations-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

给定一个可以包含重复元素的整数数组，仅查找所有不同的排列。

示例

```go
Input: [2, 2, 1]
Output: [[2 2 1] [2 1 2] [1 2 2]]

Input: [2, 2, 1, 1]
Output: [[2 2 1 1] [2 1 2 1] [2 1 1 2] [2 1 1 2] [2 1 2 1] [1 2 2 1] [1 2 1 2] [1 1 2 2] [1 2 1 2] [1 2 2 1] [1 1 2 2]]
```

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func permuteUnique(nums []int) [][]int {
	return permuteUtil(nums, 0, len(nums), len(nums))
}

func shouldSwap(nums []int, start, index int) bool {
	for i := start; i < index; i++ {
		if nums[start] == nums[index] {
			return false
		}

	}
	return true

}
func permuteUtil(nums []int, start, end int, length int) [][]int {
	output := make([][]int, 0)
	if start == end-1 {
		return [][]int{nums}
	} else {
		for i := start; i < end; i++ {
			if shouldSwap(nums, start, i) {
				nums[start], nums[i] = nums[i], nums[start]
				n := make([]int, length)
				for k := 0; k < length; k++ {
					n[k] = nums[k]
				}
				o := permuteUtil(n, start+1, end, length)
				output = append(output, o...)
				nums[i], nums[start] = nums[start], nums[i]
			}

		}
	}
	return output
}

func main() {
	output := permuteUnique([]int{2, 2, 1})
	fmt.Println(output)

	output = permuteUnique([]int{2, 2, 1, 1})
	fmt.Println(output)
}
```

**输出**

```go
[[2 2 1] [2 1 2] [1 2 2]]
[[2 2 1 1] [2 1 2 1] [2 1 1 2] [2 1 1 2] [2 1 2 1] [1 2 2 1] [1 2 1 2] [1 1 2 2] [1 2 1 2] [1 2 2 1] [1 1 2 2]]
```

**注意：** 查看我们的Golang高级教程。本系列教程内容详尽，我们尽量用示例覆盖所有概念。此教程适合希望获得Golang专业知识和深入理解的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章就是为你准备的 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
