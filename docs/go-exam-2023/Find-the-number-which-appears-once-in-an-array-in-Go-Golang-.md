<!--yml

类别: 未分类

日期: 2024-10-13 06:50:25

-->

# 在 Go(Golang) 中找到数组中只出现一次的数字

> 来源：[https://golangbyexample.com/number-array-once-golang/](https://golangbyexample.com/number-array-once-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

给定一个数组，其中每个元素出现两次，只有一个元素出现一次。目标是以常量额外空间找到该元素

**示例 1**

```
Input: [2, 1, 2, 3, 3]
Output: 1
```

**示例 2**

```
Input: [1, 1, 4]
Output: 4
```

这里的思路是使用 XOR。我们将利用 XOR 的两个特性

+   一个数字与其自身进行 XOR 结果是 0

+   0 和任何数字的 XOR 结果是该数字

所以这个思路是对数组中的所有数字进行 XOR。最终得到的数字就是答案。

# **程序**

以下是相应的程序

```
package main

import "fmt"

func singleNumber(nums []int) int {
	lenNums := len(nums)
	res := 0
	for i := 0; i < lenNums; i++ {
		res = res ^ nums[i]
	}

	return res
}

func main() {
	output := singleNumber([]int{2, 1, 2, 3, 3})
	fmt.Println(output)

	output = singleNumber([]int{1, 1, 4})
	fmt.Println(output)

}
```

**输出：**

```
1
4
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详细，我们尽力涵盖所有概念及示例。这个教程适合那些希望获得专业知识和扎实理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解如何在 Golang 中实现所有设计模式感兴趣。那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

同时，请查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)*
