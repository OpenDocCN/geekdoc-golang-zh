<!--yml

类别：未分类

日期：2024-10-13 06:43:29

-->

# 在 Go（Golang）中查找整数数组中第一个缺失的正整数

> 来源：[`golangbyexample.com/first-missing-postivie-integer-golang/`](https://golangbyexample.com/first-missing-postivie-integer-golang/)

目录

**   概述

+   程序*  *## **概述**

目标是找到整数数组中第一个缺失的正整数。

示例

```go
Input: [3, 2, -2]
Output: 1

Input: [7, 8, 9, 11, 12]
Output: 1

Input: [1, 2, 3]
Output: 4
```

如果数组中存在与该索引相等的数字，我们可以将该索引的值变为负数。对于这个问题，零和负数不重要，所以我们将正数放在一边，零/负数放在另一边。

然后我们仅对正数进行操作，并遵循以下方法。

+   我们遍历数组。对于当前元素**x**，我们将**arr[x-1]**的值改为负数。

+   最后，我们遍历并返回仍为正数的索引。如果所有索引都是负数，则返回

```go
total_num_of_positive_int + 1
```

## **程序**

以下是相同的程序

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	output := firstMissingPositive([]int{3, 2, -2})
	fmt.Println(output)

	output = firstMissingPositive([]int{-3, -2, -1})
	fmt.Println(output)

	output = firstMissingPositive([]int{7, 8, 9, 11, 12})
	fmt.Println(output)

	output = firstMissingPositive([]int{1, 2, -1})
	fmt.Println(output)

	output = firstMissingPositive([]int{1, 2, 3})
	fmt.Println(output)

	output = firstMissingPositive([]int{1, 1})
	fmt.Println(output)
}

func firstMissingPositive(nums []int) int {

	onlyPositiveNumsArray, k := segregate((nums))

	for i := 0; i < k; i++ {
		value := int(math.Abs(float64(onlyPositiveNumsArray[i])))

		if value > 0 && value <= k {
			if onlyPositiveNumsArray[value-1] > 0 {
				onlyPositiveNumsArray[value-1] = -1 * onlyPositiveNumsArray[value-1]
			}

		}
	}

	for i := 0; i < k; i++ {
		if onlyPositiveNumsArray[i] > 0 {
			return i + 1
		}
	}

	return k + 1
}

func segregate(nums []int) ([]int, int) {

	k := 0

	for i := 0; i < len(nums); i++ {
		if nums[i] > 0 {
			nums[k] = nums[i]
			k++
		}
	}

	return nums[0:k], k

}
```

**输出**

```go
1
1
1
3
4
2
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程详细且我们尝试涵盖所有概念及示例。这个教程是为那些希望获得 Golang 专业知识和扎实理解的人准备的 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
