<!--yml

类别：未分类

日期：2024-10-13 06:51:02

-->

# Go (Golang) 中的最长连续序列程序

> 来源：[`golangbyexample.com/longest-consecutive-sequence-golang/`](https://golangbyexample.com/longest-consecutive-sequence-golang/)

目录

**   概述

+   程序*  *# **概述**

给定一个整数数组。找到其中最长连续序列的长度。让我们看看一个示例来理解它。

**示例 1**

```go
Input: [4,7,3,8,2,1]
Output: 4
Reason: The longest consecutive sequence is [1,2,3,4]
```

**示例 2**

```go
Input: [4,7,3,8,2,1,9,24,10,11]
Output: 5
Reason: The longest consecutive sequence is [7,8,9,10,11]
```

天真的想法是对数组进行排序并返回最长的连续序列。但我们可以在 O(n) 时间内完成这个任务。想法是使用哈希表。下面是这个思路。

+   将每个数字存储在哈希表中。

+   然后从每个数字开始，找到以该数字开头的最长连续序列的长度。如果一个数字是 n，我们尝试在哈希中找到 n+1, n+2… 并获取从该数字开始的最长连续序列的长度。

+   如果步骤 2 中找到的长度大于之前找到的最长连续序列长度，则更新最长连续序列长度。

# **程序**

下面是相应的程序。

```go
package main

import "fmt"

func longestConsecutive(nums []int) int {
	numsMap := make(map[int]int)

	lenNums := len(nums)

	for i := 0; i < lenNums; i++ {
		numsMap[nums[i]] = 0
	}

	maxLCS := 0
	for i := 0; i < lenNums; i++ {
		currentLen := 1
		counter := 1

		for {
			val, ok := numsMap[nums[i]+counter]
			if ok {
				if val != 0 {
					currentLen += val
					break
				} else {
					currentLen += 1
					counter += 1
				}
			} else {
				break
			}
		}

		if currentLen > maxLCS {
			maxLCS = currentLen
		}
		numsMap[nums[i]] = currentLen
	}

	return maxLCS
}

func main() {
	output := longestConsecutive([]int{4, 7, 3, 8, 2, 1})
	fmt.Println(output)

	output = longestConsecutive([]int{4, 7, 3, 8, 2, 1, 9, 24, 10, 11})
	fmt.Println(output)

}
```

**输出：**

```go
4
5
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力覆盖所有概念及示例。此教程适合那些希望获得专业知识和对 Golang 有扎实理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现，如果是的话，这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

另外，可以在这里查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
