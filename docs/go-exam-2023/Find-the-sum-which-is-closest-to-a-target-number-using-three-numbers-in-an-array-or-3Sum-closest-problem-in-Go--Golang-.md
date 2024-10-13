<!--yml

类别：未分类

日期：2024-10-13 06:42:39

-->

# 使用数组中的三个数字找到最接近目标数字的和，或者说是 Go 语言中的 3Sum 最接近问题。

> 来源：[`golangbyexample.com/three-sum-closest-golang/`](https://golangbyexample.com/three-sum-closest-golang/)

目录

+   概述

+   程序 

## **概述**

目标是使用给定数组中的三个三元组找到最接近给定目标和的和。

对于

```go
Input Array: [0,2,3,-1] Target Sum:6
Output: 5 . It can be formed using 0+2+3 
```

## **程序**

以下是相同的程序

```go
package main

import (
	"fmt"
	"math"
	"sort"
)

func main() {
	output := threeSumClosest([]int{0, 2, 3, -1}, 6)
	fmt.Println(output)
}

func threeSumClosest(nums []int, target int) int {

	numsLength := len(nums)

	closestSum := 0
	nearest := 10000

	sort.Slice(nums, func(i, j int) bool {
		return nums[i] < nums[j]
	})

	for k := 0; k < numsLength-2; k++ {
		i := k + 1
		j := numsLength - 1
		for i < j {
			sum := nums[k] + nums[i] + nums[j]
			absSum := int(math.Abs(float64(target - sum)))

			if absSum < nearest {
				nearest = absSum
				closestSum = sum
			}

			if nums[k]+nums[i]+nums[j] > target {
				j--
			} else {
				i++
			}
		}
	}

	return closestSum
}
```

**输出**

```go
5
```

**注意：** 请查看我们的 Golang 高级教程。此系列的教程内容详尽，我们尽量用例子覆盖所有概念。本教程适合那些希望获得 Golang 专业知识和扎实理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [去](https://golangbyexample.com/tag/go/) *   [Golang](https://golangbyexample.com/tag/golang/)*
