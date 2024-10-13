<!--yml

category: 未分类

日期：2024-10-13 06:47:59

-->

# 在 Go 中合并两个已排序数组（Golang）

> 来源：[`golangbyexample.com/merge-two-sorted-arrays-golang/`](https://golangbyexample.com/merge-two-sorted-arrays-golang/)

目录

**   概述

+   程序*  *## **概述**

给定两个数组。两个数组均已排序。

+   第一个数组的长度为 m+n

+   第二个数组的长度为 n

目标是合并这些已排序的数组。第一个数组的长度足够，因此只需修改第一个数组

```go
Input1: [2,3,4,0,0]
Input2: [1,5]
Output: [1, 2, 3, 4, 5]

Input1: [4,5,0,0,0,0]
Input2: [1, 2, 3, 7]
Output: [1, 2, 3, 4, 5, 7]
```

这是我们可以采取的方法

+   将第一个数组中的所有元素按排序顺序移动到末尾。第一个数组将变为

```go
[0,0,2,3,4]
```

+   现在从第**m**个索引元素开始，第一个数组的**0**个索引在第二个数组中。

+   比较两个数组，并将较小的放置在第**0**个索引中。第一个数组将变为

```go
[1, 0, 2, 3, 4]
```

+   重复此过程。第一个数组末尾的值将在覆盖之前放置在前面，因为我们有足够的空间

## **程序**

这是相应的程序。

```go
package main

import "fmt"

func merge(nums1 []int, m int, nums2 []int, n int) []int {

	if m == 0 {
		for k := 0; k < n; k++ {
			nums1[k] = nums2[k]
		}
		return nums1
	}
	nums1 = moveToEnd(nums1, m)
	i := n
	j := 0
	for k := 0; k < m+n; k++ {
		if i < m+n && j < n {
			if nums1[i] < nums2[j] {
				nums1[k] = nums1[i]
				i++
			} else {
				nums1[k] = nums2[j]
				j++
			}
		} else if j < n {
			nums1[k] = nums2[j]
			j++
		}

	}

	return nums1

}

func moveToEnd(nums []int, m int) []int {
	lenNums := len(nums)

	k := lenNums

	for i := m - 1; i >= 0; i-- {
		nums[k-1] = nums[i]
		k--
	}

	return nums
}

func main() {
	output := merge([]int{2, 3, 4, 0, 0}, 3, []int{1, 5}, 2)
	fmt.Println(output)

	output = merge([]int{4, 5, 0, 0, 0, 0}, 2, []int{1, 2, 3, 7}, 4)
	fmt.Println(output)
}
```

**输出**

```go
[1 2 3 4 5]
[1 2 3 4 5 7]
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程详细阐述了所有概念，并附有示例。该教程适合希望获得专业知识和扎实理解 Golang 的学习者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果您有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章就是为您准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
