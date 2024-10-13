<!--yml

类别：未分类

日期：2024-10-13 06:47:08

-->

# Go（Golang）中的范围和数组程序

> 来源：[`golangbyexample.com/range-sum-array-golang/`](https://golangbyexample.com/range-sum-array-golang/)

目录

+   概述

+   程序 

## **概述**

有一个给定的数字数组。目标是在该数组中找到范围和。这意味着将给定一个范围，其中包含左索引和右索引。我们必须在给定的数字数组中找到左索引和右索引之间的和。

看起来很简单，对吧？只需从给定数组的左索引迭代到右索引并返回和。但这里有个陷阱。允许的时间复杂度是 O(1)

这里是我们可以遵循的方法，以便能够在 O(1)时间复杂度内返回答案

+   从给定的数字数组预计算另一个 sum_array

+   sum_array[i] = 从 0 索引到第 i 索引的数字之和。

+   对于给定的左索引和右索引，只需返回 sum_array[left-1] – sum_array[right]。当然，我们需要验证 left-1 是否大于或等于零。

## **程序**

这是相同程序的实现。

```go
package main

import "fmt"

type NumArray struct {
	sum_nums []int
}

func initNumArray(nums []int) NumArray {
	length := len(nums)
	sum_nums := make([]int, length)

	sum_nums[0] = nums[0]

	for i := 1; i < length; i++ {
		sum_nums[i] = nums[i] + sum_nums[i-1]
	}

	return NumArray{
		sum_nums: sum_nums,
	}
}

func (this *NumArray) SumRange(left int, right int) int {
	leftSum := 0
	if left > 0 {
		leftSum = this.sum_nums[left-1]
	}
	return this.sum_nums[right] - leftSum
}

func main() {
	nums := []int{1, 3, 5, 6, 2}
	na := initNumArray(nums)

	output := na.SumRange(2, 4)
	fmt.Println(output)

}
```

**输出**

```go
13
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容丰富，涵盖了所有概念及其示例。本教程适合那些希望获得专业知识和对 Golang 有扎实理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解如何在 Golang 中实现所有设计模式感兴趣，那么这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
