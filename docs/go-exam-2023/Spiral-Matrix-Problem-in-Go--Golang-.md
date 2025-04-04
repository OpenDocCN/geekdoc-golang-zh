<!--yml

分类：未分类

日期：2024-10-13 06:44:15

-->

# Go 语言中的螺旋矩阵问题

> 来源：[`golangbyexample.com/spiral-matrix-problem-golang/`](https://golangbyexample.com/spiral-matrix-problem-golang/)

目录

+   概述

+   程序

## **概述**

目标是以螺旋格式打印矩阵。

例如

**输入**

```go
1, 2, 3 
4, 5, 6 
7, 8, 9
```

**输出**

```go
1 2 3 6 9 8 7 4 5
```

## **程序**

以下是相应的程序

```go
package main

import "fmt"

func main() {
	matrix := [][]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
	output := spiralOrder(matrix)
	fmt.Println(output)

	matrix = [][]int{{1, 2, 3, 4}, {5, 6, 7, 8}, {9, 10, 11, 12}, {13, 14, 15, 16}}
	output = spiralOrder(matrix)
	fmt.Println(output)
}

func spiralOrder(matrix [][]int) []int {

	var output []int
	totalRows := len(matrix)
	totalColumns := len(matrix[0])

	startRow := 0
	endRow := totalRows - 1
	startColumn := 0
	endColumn := totalColumns - 1

	for startRow <= endRow && startColumn <= endColumn {

		for i := startColumn; i <= endColumn; i++ {
			output = append(output, matrix[startRow][i])
		}

		startRow++

		for i := startRow; i <= endRow; i++ {
			output = append(output, matrix[i][endColumn])
		}

		endColumn--

		if startRow <= endRow {
			for i := endColumn; i >= startColumn; i-- {
				output = append(output, matrix[endRow][i])
			}
		}

		endRow--

		if startColumn <= endColumn {
			for i := endRow; i >= startRow; i-- {
				output = append(output, matrix[i][startColumn])
			}
		}
		startColumn++
	}

	return output

}
```

**输出**

```go
[1 2 3 6 9 8 7 4 5]
[1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10]
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽量用例子覆盖所有概念。这个教程适合那些希望获得专业知识和对 golang 有深入理解的读者——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

如果你有兴趣了解所有设计模式如何在 Golang 中实现，那么这篇文章就是为你准备的——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)。


