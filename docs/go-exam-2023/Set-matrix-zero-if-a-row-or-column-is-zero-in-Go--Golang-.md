<!--yml

分类：未分类

日期：2024-10-13 06:48:56

-->

# 在 Go (Golang)中，如果行或列为零，则设置矩阵为零

> 来源：[`golangbyexample.com/set-matrix-zero-golang/`](https://golangbyexample.com/set-matrix-zero-golang/)

目录

+   概述

+   程序 *  * ## **概述**

给定一个 m*n 的矩阵。如果一个元素为零，则将其行和列设置为零

示例

**输入：**

```go
1, 1, 1 
0, 1, 1 
1, 1, 1
```

**输出：**

```go
0, 1, 1 
0, 0, 0 
0, 1, 1
```

我们将通过使用长度分别为**m**和**n**的两个额外数组**rowSet**和**columnSet**来解决这个问题。我们将遍历矩阵并

```go
if matrix[i][j] == 0 then
   rowSet[i] = 1
   columnSet[j] = 1
```

如果**rowSet[i]**等于 1 或**columnSet[j]**等于 1，则可以将**matrix[i][j]**设为零。

## **程序**

这是相应的程序。

```go
package main

import "fmt"

func setZeroes(matrix [][]int) [][]int {
	numRows := len(matrix)

	numColumns := len(matrix[0])

	rowSet := make([]int, numRows)
	columnSet := make([]int, numColumns)

	for i := 0; i < numRows; i++ {
		for j := 0; j < numColumns; j++ {
			if matrix[i][j] == 0 {
				rowSet[i] = 1
				columnSet[j] = 1
			}
		}
	}

	for i := 0; i < numRows; i++ {
		for j := 0; j < numColumns; j++ {
			if rowSet[i] == 1 || columnSet[j] == 1 {

				matrix[i][j] = 0

			}
		}
	}

	return matrix

}

func main() {
	matrix := [][]int{{1, 1, 1}, {0, 1, 1}, {1, 1, 1}}
	output := setZeroes(matrix)
	fmt.Println(output)
}
```

**输出**

```go
[[0 1 1] [0 0 0] [0 1 1]]
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尝试用实例覆盖所有概念。此教程适合那些希望获得 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解如何在 Golang 中实现所有设计模式感兴趣。那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
