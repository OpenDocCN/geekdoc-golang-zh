<!--yml
category: 未分类
date: 2024-10-13 06:44:20
-->

# Rotate a symmetric matrix or an image in place in a clockwise direction in Go (Golang)

> 来源：[https://golangbyexample.com/rotate-image-clockwise-golang/](https://golangbyexample.com/rotate-image-clockwise-golang/)

**Overview**

Given an image represented in the form of a matrix, rotate this matrix or image in a clockwise direction.

For eg

**Input:**

```
[[1, 2, 3], 
 [4, 5, 6], 
 [7, 8, 9]]
```

**Output:**

```
7, 4, 1 
8, 5, 2 
9, 6, 3
```

Idea is to iterate over all the borders one by one and in place swap each side. The outer border of the above matrix is

```
7 4  1
8     2 
9 6 3
```

We can rotate matrix in place in this way

```
7->1->3->9
4->2->6->8
```

A symmetric matrix of size n*n size will have **n-1** borders. For eg above matrix has two borders

First border

```
7 4 1
8    2 
9 6 3
```

Second border

```
5
```

Table of Contents

 **   [Program](#Program "Program")*  *## **Program**

Below is the program for the same

```
package main

import "fmt"

func main() {
	matrix := [][]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
	rotate(matrix)

	matrix = [][]int{{1, 2, 3, 4}, {5, 6, 7, 8}, {9, 10, 11, 12}, {13, 14, 15, 16}}
	rotate(matrix)
}

func rotate(matrix [][]int) {

	matrixSize := len(matrix)

	startRow := 0
	endRow := matrixSize - 1
	startColumn := 0
	endColumn := matrixSize - 1
	for i := 0; i < matrixSize; i++ {
		totalCycles := endRow - startRow

		for j := 0; j < totalCycles; j++ {
			temp := matrix[startRow][startColumn+j]
			matrix[startRow][startColumn+j] = matrix[endRow-j][startColumn]

			matrix[endRow-j][startColumn] = matrix[endRow][endColumn-j]

			matrix[endRow][endColumn-j] = matrix[startRow+j][endColumn]

			matrix[startRow+j][endColumn] = temp
		}

		startRow = startRow + 1
		endRow = endRow - 1
		startColumn = startColumn + 1
		endColumn = endColumn - 1
	}

	fmt.Println(matrix)

}
```

**Output**

```
[[7 4 1] [8 5 2] [9 6 3]]
[[13 9 5 1] [14 10 6 2] [15 11 7 3] [16 12 8 4]]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*