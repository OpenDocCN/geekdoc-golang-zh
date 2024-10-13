<!--yml

分类：未分类

日期：2024-10-13 06:51:07

-->

# Go (Golang) 中的帕斯卡三角形程序

> 来源：[`golangbyexample.com/program-pascal-triangle-golang/`](https://golangbyexample.com/program-pascal-triangle-golang/)

目录

+   概述

+   程序

## **概述**

目标是打印 n 行帕斯卡三角形。数字 n 作为输入提供给程序

**示例 1**

```go
Input: numRows = 4
Output: [[1],[1,1],[1,2,1],[1,3,3,1]]
```

**示例 2**

```go
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

请参考此链接以了解更多关于帕斯卡三角形的信息 - [`en.wikipedia.org/wiki/Pascal%27s_triangle`](https://en.wikipedia.org/wiki/Pascal%27s_triangle)

这里的思路是使用动态规划。

# **程序**

以下是相同的程序

```go
package main

import "fmt"

func generate(numRows int) [][]int {
	firstRow := []int{1}

	if numRows == 1 {
		return [][]int{firstRow}
	}

	secondRow := []int{1, 1}

	if numRows == 2 {
		return [][]int{firstRow, secondRow}
	}

	output := [][]int{firstRow, secondRow}

	for i := 2; i < numRows; i++ {
		temp := make([]int, i+1)

		lastRow := output[i-1]

		temp[0] = 1
		temp[i] = 1

		for j := 1; j < i; j++ {
			temp[j] = lastRow[j-1] + lastRow[j]
		}

		output = append(output, temp)

	}

	return output

}

func main() {
	output := generate(4)
	fmt.Println(output)

	output = generate(5)
	fmt.Println(output)
}
```

**输出：**

```go
[1] [1 1] [1 2 1] [1 3 3 1]]
[[1] [1 1] [1 2 1] [1 3 3 1] [1 4 6 4 1]]
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们努力涵盖所有概念并提供示例。本教程适合那些希望获得专业知识并深入理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

此外，还可以查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
