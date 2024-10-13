<!--yml

类别：未分类

日期：2024-10-13 06:47:13

-->

# Golang 中的范围和二维数组程序

> 来源：[`golangbyexample.com/range-sum-2d-array-go/`](https://golangbyexample.com/range-sum-2d-array-go/)

目录

+   概述

+   程序

## **概述**

给定一个数字的二维矩阵。目标是计算矩阵中由**左上角**（row1, col1）和**右下角**（row2, col2）定义的矩形内元素的**和**。

看起来简单，对吧。只需从左角迭代到右角并返回总和。但这里有个问题。允许的时间复杂度是 O(1)

这里是我们可以遵循的方法，以便能够在 O(1)时间复杂度内返回答案

+   为该二维矩阵预计算另一个 sum_array

+   sum_array[i][j] = 从**左角**（0, 0）和**右角**（i, j）计算的数字之和。

+   对于给定的左角（row1, col1）和右角（row2, col2），计算

```go
topSum = sum_matrix[row1-1][col2]
leftSum = sum_matrix[row2][col1-1]
cornerSum = sum_matrix[row1-1][col1-1]
```

+   然后返回

```go
sum_matrix[row2][col2] - topSum - leftSum + cornerSum
```

## **程序**

这里是相同的程序。

```go
package main

import "fmt"

type NumMatrix struct {
	matrix     [][]int
	sum_matrix [][]int
	numColumn  int
	numRows    int
}

func initNumArray(matrix [][]int) NumMatrix {
	numRows := len(matrix)
	numColumn := len(matrix[0])

	if numColumn == 0 || numRows == 0 {
		return NumMatrix{}
	}

	sum_matrix := make([][]int, numRows)

	for i := 0; i < numRows; i++ {
		sum_matrix[i] = make([]int, numColumn)
	}

	sum_matrix[0][0] = matrix[0][0]
	for i := 1; i < numRows; i++ {
		sum_matrix[i][0] = matrix[i][0] + sum_matrix[i-1][0]
	}

	for i := 1; i < numColumn; i++ {
		sum_matrix[0][i] = matrix[0][i] + sum_matrix[0][i-1]
	}

	for i := 1; i < numRows; i++ {
		for j := 1; j < numColumn; j++ {
			sum_matrix[i][j] = matrix[i][j] + sum_matrix[i-1][j] + sum_matrix[i][j-1] - sum_matrix[i-1][j-1]
		}
	}

	num_matrix := NumMatrix{
		matrix:     matrix,
		sum_matrix: sum_matrix,
		numColumn:  numColumn,
		numRows:    numRows,
	}
	return num_matrix
}

func (this *NumMatrix) SumRegion(row1 int, col1 int, row2 int, col2 int) int {

	topSum := 0
	leftSum := 0
	cornerSum := 0
	if row1 > 0 {
		topSum = this.sum_matrix[row1-1][col2]
	}

	if col1 > 0 {
		leftSum = this.sum_matrix[row2][col1-1]
	}

	if row1 > 0 && col1 > 0 {
		cornerSum = this.sum_matrix[row1-1][col1-1]
	}

	return this.sum_matrix[row2][col2] - topSum - leftSum + cornerSum
}

func main() {
	matrix := [][]int{{1, 3, 5}, {6, 7, 4}}
	na := initNumArray(matrix)

	output := na.SumRegion(0, 1, 1, 2)
	fmt.Println(output)

}
```

**输出**

```go
19
```

**注意：** 查看我们的 Golang 高级教程。本系列的教程内容详尽，我们努力涵盖所有概念及其示例。本教程适合那些希望获得专业知识并深入理解 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
