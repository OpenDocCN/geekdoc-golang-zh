<!--yml

类别: 未分类

日期：2024-10-13 06:46:28

-->

# Go语言中的最长递增子序列程序

> 来源：[https://golangbyexample.com/longest-increasing-subsequence-golang/](https://golangbyexample.com/longest-increasing-subsequence-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是找到给定输入数组中的最长递增子序列。它是给定序列中最长的子序列，使得每个元素都大于其前一个元素。

例如

```
Input: [1,5,7,6]
The longest subsequence is [1,5,6] which is of length 3
Output: 3
```

另一个例子

```
Input: [3,2,1]
The longest subsequence is either {3}, {2} or {1}. Each is of length 1
Output: 1
```

最长递增子序列是一个动态规划问题。假设输入数组仅命名为 **input**。假设 **lis** 是一个数组，其中 **lis[i]** 是索引 **i** 处的最长递增子序列的长度。

然后

+   **lis[0]** = 1

+   **lis[i]** = **max(lis[j])** + 1 其中 0 <= **j** < **i** 且 **input[i]** > **input[j]**

+   **lis[i]** = 1 如果没有这样的 **j**

## **程序**

这里是相同程序的代码。

```
package main

import "fmt"

func lengthOfLIS(nums []int) int {

	lenNums := len(nums)
	lis := make([]int, lenNums)

	for i := 0; i < lenNums; i++ {
		lis[i] = 1
	}

	for i := 1; i < lenNums; i++ {
		for j := 0; j < i; j++ {
			if nums[i] > nums[j] && lis[i] < (lis[j]+1) {
				lis[i] = lis[j] + 1
			}
		}
	}

	max := 0

	for i := 0; i < lenNums; i++ {
		if lis[i] > max {
			max = lis[i]
		}
	}

	return max
}

func main() {
	output := lengthOfLIS([]int{1, 5, 7, 6})
	fmt.Println(output)

	output = lengthOfLIS([]int{3, 2, 1})
	fmt.Println(output)
}
```

**输出**

```
3
1
```

**注意：** 请查看我们的Golang高级教程。本系列教程内容详尽，我们试图通过示例覆盖所有概念。本教程适合那些希望获得Golang专业知识和扎实理解的人 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章就是为你准备的 – [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
