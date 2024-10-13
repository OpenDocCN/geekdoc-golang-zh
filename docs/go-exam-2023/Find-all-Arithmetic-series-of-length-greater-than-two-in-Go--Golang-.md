<!--yml

分类: 未分类

日期: 2024-10-13 06:49:06

-->

# 在 Go (Golang) 中查找所有长度大于二的算术序列

> 来源：[https://golangbyexample.com/arithmetic-series-golang/](https://golangbyexample.com/arithmetic-series-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

算术序列是指每个元素之间的差值相等的序列。在此程序中，给定一个整数数组。目标是找到所有长度大于二的算术序列。

通过一个例子来最好地理解这个问题

示例

```go
Input: [2,3,4,5]
Output: 3
```

在上述数组中我们有三个长度大于 2 的算术片段

+   2,3,4

+   3,4,5

+   2,3,4,5

这是一个动态规划问题，因为它具有最优子结构。假设数组的名称为 input

+   dp[0] = 0

+   dp[1] = 0

+   dp[2] = 1 如果 dp[2] – dp[1] == dp[1] – dp[0]

+   dp[i] = 1 如果 dp[i] – dp[i-1] == dp[i-1] – dp[i-2]

其中 **dp[i]** 表示长度大于 2 的算术序列直到长度 **i+1** 的数量

## **程序**

这是相应的程序。

```go
package main

import "fmt"

func numberOfArithmeticSlices(nums []int) int {
	lenNums := len(nums)

	if lenNums <= 2 {
		return 0
	}

	dp := make([]int, lenNums)

	dp[0] = 0

	if (nums[2] - nums[1]) == nums[1]-nums[0] {
		dp[2] = 1
	}

	for i := 3; i < lenNums; i++ {
		if nums[i]-nums[i-1] == nums[i-1]-nums[i-2] {
			dp[i] = dp[i-1] + 1

		}
	}

	output := 0
	for i := 2; i < lenNums; i++ {
		output = output + dp[i]
	}

	return output

}

func main() {
	output := numberOfArithmeticSlices([]int{2, 3, 4, 5})
	fmt.Println(output)
}
```

**输出**

```go
3
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尝试用例子涵盖所有概念。本教程适合那些希望获得专业知识和扎实理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)长度大于两个的算术序列

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
