<!--yml

类别：未分类

日期：2024-10-13 06:48:04

-->

# 在 Go（Golang）中原地移除数组中的给定值的所有出现

> 来源：[`golangbyexample.com/remove-element-golang/`](https://golangbyexample.com/remove-element-golang/)

目录

**   概述

+   程序*  *## **概述**

给定一个整数数组和一个目标元素。从数组中移除所有该目标元素的出现。删除必须在原地进行。

```go
Input: [1, 4, 2, 5, 4]
Target: 4
Output: [1, 2, 5]

Input: [1, 2, 3]
Target:3
Output: [1, 2]
```

## **程序**

这里是相应的程序。

```go
package main

import (
	"fmt"
)

func removeElement(nums []int, val int) []int {
	lenNums := len(nums)

	k := 0

	for i := 0; i < lenNums; {
		if nums[i] != val {
			nums[k] = nums[i]
			k++
		}
		i++
	}
	return nums[0:k]
}

func main() {
	output := removeElement([]int{1, 4, 2, 5, 4}, 4)
	fmt.Println(output)

	output = removeElement([]int{1, 2, 3}, 3)
	fmt.Println(output)
}
```

**输出**

```go
[1 2 5]
[1 2]
```

**注意：** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试用实例覆盖所有概念。本教程适合那些希望获得专业知识和对 Golang 有深入理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*
