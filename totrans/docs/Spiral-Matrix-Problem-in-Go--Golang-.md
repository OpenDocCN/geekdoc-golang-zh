<!--yml
category: 未分类
date: 2024-10-13 06:44:15
-->

# Spiral Matrix Problem in Go (Golang)

> 来源：[https://golangbyexample.com/spiral-matrix-problem-golang/](https://golangbyexample.com/spiral-matrix-problem-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to print a matrix in a spiral format.

Eg

**Input**

```
1, 2, 3 
4, 5, 6 
7, 8, 9
```

**Output**

```
1 2 3 6 9 8 7 4 5
```

## **Program**

Below is the program for the same

```
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

**Output**

```
[1 2 3 6 9 8 7 4 5]
[1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*