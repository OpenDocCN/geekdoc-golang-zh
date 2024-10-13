<!--yml

类别：未分类

日期：2024-10-13 06:46:06

-->

# Go 中的最大和子数组程序 (Golang)

> 来源：[https://golangbyexample.com/maximum-sum-subarray-golang/](https://golangbyexample.com/maximum-sum-subarray-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是在给定的输入数组中找到最大子数组。子数组应是连续的，并且至少应包含一个元素。

例如

```
Input: [4, 5 ,-3]
Maximum Subarray is [4, 5]
Output: 9
```

另一个示例

```
Input: [1, 2, -4, 4, 1]
Maximum Subarray is [4, 1]
Output: 5
```

我们在这里使用 Kadane 算法。在 Kadane 算法中，我们保持两个变量 **max** 和 **currentMax**。

+   **max** 初始化为 IntMin，**currentMax** 初始化为零。

+   然后我们对数组中的每个元素进行循环。

    +   设置 **currentMax** = **currentMax** + a[i]。

    +   如果 **currentMax** > **max**，则将 **max** 设置为 **currentMax**。

    +   如果 **currentMax** 小于零，则将 **currentMax** 重置为零。

## **程序**

这里是相同的程序。

```
package main

import "fmt"

const (
	UintSize = 32 << (^uint(0) >> 32 & 1)
	MaxInt   = 1<<(UintSize-1) - 1 // 1<<31 - 1 or 1<<63 - 1
	MinInt   = -MaxInt - 1
)

func main() {
	input := []int{4, 5, -3}
	output := maxSubArray(input)
	fmt.Println(output)

	input = []int{1, 2, -4, 4, 1}
	output = maxSubArray(input)
	fmt.Println(output)
}

func maxSubArray(nums []int) int {
	lenNums := len(nums)

	currentMax := 0
	max := MinInt
	for i := 0; i < lenNums; i++ {
		currentMax = currentMax + nums[i]
		if currentMax > max {
			max = currentMax
		}

		if currentMax < 0 {
			currentMax = 0
		}

	}
	return max
}
```

**输出**

```
9
5
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们尝试用示例覆盖所有概念。本教程适合那些希望获得专业知识和扎实理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
