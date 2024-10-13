<!--yml

分类：未分类

日期：2024-10-13 06:44:20

-->

# 在 Go (Golang)中顺时针旋转对称矩阵或图像

> 来源：[`golangbyexample.com/rotate-image-clockwise-golang/`](https://golangbyexample.com/rotate-image-clockwise-golang/)

**概述**

给定一个以矩阵形式表示的图像，将该矩阵或图像顺时针旋转。

例如

**输入：**

```go
[[1, 2, 3], 
 [4, 5, 6], 
 [7, 8, 9]]
```

**输出：**

```go
7, 4, 1 
8, 5, 2 
9, 6, 3
```

思路是逐一遍历所有边界，并就地交换每一侧。上述矩阵的外边界是

```go
7 4  1
8     2 
9 6 3
```

我们可以这样就地旋转矩阵

```go
7->1->3->9
4->2->6->8
```

一个 n*n 大小的对称矩阵将有**n-1**个边界。例如，上述矩阵有两个边界

第一个边界

```go
7 4 1
8    2 
9 6 3
```

第二个边界

```go
5
```

目录

**   程序*  *## **程序**

下面是相应的程序

```go
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

**输出**

```go
[[7 4 1] [8 5 2] [9 6 3]]
[[13 9 5 1] [14 10 6 2] [15 11 7 3] [16 12 8 4]]
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力用例子覆盖所有概念。这个教程适合那些希望获得专业知识和对 Golang 有深入理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
