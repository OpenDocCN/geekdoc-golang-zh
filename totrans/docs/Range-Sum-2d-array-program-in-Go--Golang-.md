<!--yml
category: 未分类
date: 2024-10-13 06:47:13
-->

# Range Sum 2d array program in Go (Golang)

> 来源：[https://golangbyexample.com/range-sum-2d-array-go/](https://golangbyexample.com/range-sum-2d-array-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

There is a 2d matrix of numbers that is given. The objective Calculate the **sum** of the elements of matrix inside the rectangle defined by its **upper left corner** (row1, col1) and **lower right corner** (row2, col2).

Seem simple right. Just iterate from the left corner to the right corner in the given array and return the sum. But here is the catch. Allowed TC is O(1)

Here is the approach we can follow so that we are able to return the answer in O(1) time complexity

*   Pre compute another sum_array for that 2d matrix

*   sum_array[i][j] = sum of numbers from the **left corner** (0, 0) and **right corner** (i, j).

*   For a given left corner (row1, col1) and right corner (row2, col2) compute

```
topSum = sum_matrix[row1-1][col2]
leftSum = sum_matrix[row2][col1-1]
cornerSum = sum_matrix[row1-1][col1-1]
```

*   Then return

```
sum_matrix[row2][col2] - topSum - leftSum + cornerSum
```

## **Program**

Here is the program for the same.

```
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

**Output**

```
19
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*