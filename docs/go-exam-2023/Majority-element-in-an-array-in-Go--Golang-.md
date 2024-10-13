<!--yml

类别：未分类

日期：2024-10-13 06:50:20

-->

# 在Go (Golang)中找到数组中的多数元素。

> 来源：[https://golangbyexample.com/majority-element-array-golang/](https://golangbyexample.com/majority-element-array-golang/)。

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

目标是找到给定数组中的多数元素。多数元素是指在给定数组中出现次数超过 n/2 的元素，其中 n 是数组的长度。

**示例 1**

```go
Input: [2, 1, 2, 2, 3]
Output: 2
```

**示例 2**

```go
Input: [1]
Output: 1
```

# **程序**

下面是相同内容的程序。

```go
package main

import "fmt"

func majorityElement(nums []int) int {
	lenNums := len(nums)

	if lenNums == 1 {
		return nums[0]
	}

	numsMap := make(map[int]int)

	for i := 0; i < lenNums; i++ {
		_, ok := numsMap[nums[i]]
		if ok {
			numsMap[nums[i]] = numsMap[nums[i]] + 1
			if numsMap[nums[i]] > lenNums/2 {
				return nums[i]
			}
		} else {
			numsMap[nums[i]] = 1
		}
	}

	return 0

}

func main() {
	output := majorityElement([]int{2, 1, 2, 2, 3})
	fmt.Println(output)

	output = majorityElement([]int{1})
	fmt.Println(output)
}
```

**输出：**

```go
2
1
```

**注意：** 请查看我们的Golang进阶教程。本系列的教程内容详尽，我们尝试用例子覆盖所有概念。本教程适合那些希望获得专业知识和对Golang有扎实理解的人—— [Golang进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

如果你有兴趣了解如何在Golang中实现所有设计模式。如果是，那么这篇文章适合你—— [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)。

此外，请查看我们的系统设计教程系列—— [系统设计教程系列](https://techbyexample.com/system-design-questions/)。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
