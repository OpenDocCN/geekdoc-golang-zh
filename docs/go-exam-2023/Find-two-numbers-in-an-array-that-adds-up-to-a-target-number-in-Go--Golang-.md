<!--yml

分类：未分类

日期：2024-10-13 06:41:48

-->

# 在 Go (Golang) 中查找两个数组中的数字，使它们的和等于目标数字

> 来源：[https://golangbyexample.com/target-sum-golang/](https://golangbyexample.com/target-sum-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

例如，假设我们有一个给定的数组

```go
[2, 5, 1, 3]
```

目标数字是 **4**

那么答案将是索引

```go
[2, 3]
```

因为我们有

+   索引 2 的数字 1

+   索引 3 的数字 3

并且 1+3 = 4

请注意数组是无序的

预期时间复杂度 – O(n)

我们可以使用哈希来解决这个问题。它的基础思想是

+   如果说其中一个数字是 **x**

+   那么另一个数字将是 **target-x**

因此，如果对于一个数字 **x**，我们检查 **target-x** 是否在哈希中。如果在，那么我们知道我们找到了答案

我们来看一个相同的程序。

## **程序**

```go
package main

import "fmt"

func main() {
	output := twoTargetSums([]int{2, 5, 1, 3}, 4)
	fmt.Println(output)
}

func twoTargetSums(nums []int, target int) []int {
	numberMap := make(map[int]int)
	output := make([]int, 2)
	for i := 0; i < len(nums); i++ {
		val, ok := numberMap[target-nums[i]]
		if ok {
			output[0] = val
			output[1] = i
			return output
		} else {
			numberMap[nums[i]] = i
		}
	}
	return output
}
```

**输出**

```go
[2 3]
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
