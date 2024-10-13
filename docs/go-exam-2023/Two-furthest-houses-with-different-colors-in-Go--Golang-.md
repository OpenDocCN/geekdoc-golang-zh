<!--yml

分类：未分类

日期：2024-10-13 06:49:42

-->

# Go 语言中颜色不同的最远房子

> 来源：[`golangbyexample.com/two-furthest-houses-different-color-golang/`](https://golangbyexample.com/two-furthest-houses-different-color-golang/)

目录

+   概述

+   程序

## **概述**

给定一个数组，表示房子的颜色。所以 array[i]表示索引 i 处房子的颜色。目标是找到两个颜色不同且距离最远的房子。

如果不存在这样的索引，则返回-1。

示例 1

```go
Input: intervals = [2,2,2,1,2,2]
Output: 3
Explanation: House at index 0 and house at index 3 is of different colors
```

示例 2

```go
Input: intervals = [1, 2 ,3, 1, 2]
Output: 2
Explanation: House at index 0 and house at index 4 is of different colors
```

以下是我们可以采取的方法。

+   其中一个房子将位于第 0 个索引或 n-1 索引处，其中 n 是数组的长度。

+   我们可以先假设第 0 个索引的房子在解决方案中，然后进行计算。

+   我们可以接着假设 n-1 索引的房子在解决方案中，然后进行计算。

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func maxDistance(colors []int) int {
	lenColors := len(colors)

	if lenColors == 0 || lenColors == 1 {
		return 0
	}
	maxDiff := 0

	leftColor := colors[0]
	rightColor := colors[lenColors-1]

	for i := 1; i < lenColors; i++ {
		if colors[i] != leftColor {
			maxDiff = i
		}
	}

	for i := lenColors - 2; i >= 0; i-- {
		if colors[i] != rightColor {
			diff := lenColors - i - 1
			if diff > maxDiff {
				maxDiff = diff
			}
		}
	}

	return maxDiff
}
func main() {
	output := maxDistance([]int{2, 2, 2, 1, 2, 2})
	fmt.Println(output)

	output = maxDistance([]int{1, 2, 3, 1, 2})
	fmt.Println(output)
}
```

**输出**

```go
4
-1
```

**注意：** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尽力涵盖所有概念和示例。这个教程适合那些希望获得 Golang 专业知识和扎实理解的人——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章就是为你准备的——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*
