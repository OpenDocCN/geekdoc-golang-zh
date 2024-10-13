<!--yml

分类：未分类

日期：2024-10-13 06:49:01

-->

# 在 Go 中查找数组中的所有重复项 (Golang)

> 来源：[`golangbyexample.com/find-all-duplicates-array-golang/`](https://golangbyexample.com/find-all-duplicates-array-golang/)

目录

**   概述

+   程序*  *## **概述**

给定一个数组，其中所有元素的范围在[1, n]内，n 是数组的长度。目标是找到该数组中的所有重复项。

示例

```go
Input: [1, 2, 3, 2, 4, 3]
Output: [2, 3]
```

这里的想法是利用数字在范围[1, n]内的事实。对于数组中的每个元素，增加其索引处的值 n。因此

+   要获取索引处的值，我们用 value%n

+   最终，如果任何索引的值大于 2*n，那么它是重复的。

## **程序**

这是相同的程序。

```go
package main

import "fmt"

func findDuplicates(nums []int) []int {
	lenNums := len(nums)

	for i := 0; i < lenNums; i++ {
		index := (nums[i] - 1) % lenNums

		nums[index] = lenNums + nums[index]
	}

	k := 0

	for i := 0; i < lenNums; i++ {
		if nums[i] > 2*lenNums {
			nums[k] = i + 1
			k++
		}
	}

	return nums[0:k]

}

func main() {
	output := findDuplicates([]int{1, 2, 3, 2, 4, 3})
	fmt.Println(output)
}
```

**输出**

```go
[2 3]
```

**注意：** 请查看我们的 Golang 高级教程。该系列的教程内容详尽，我们努力涵盖所有概念及示例。本教程适合那些希望获得专业知识和深入理解 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
