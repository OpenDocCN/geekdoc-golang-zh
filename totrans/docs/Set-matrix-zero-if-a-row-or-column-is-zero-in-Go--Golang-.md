<!--yml
category: 未分类
date: 2024-10-13 06:48:56
-->

# Set matrix zero if a row or column is zero in Go (Golang)

> 来源：[https://golangbyexample.com/set-matrix-zero-golang/](https://golangbyexample.com/set-matrix-zero-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An m*n matrix is given. If an element is zero then set its row and column to zero

Examples

**Input:**

```
1, 1, 1 
0, 1, 1 
1, 1, 1
```

**Output:**

```
0, 1, 1 
0, 0, 0 
0, 1, 1
```

We will solve it by taking two additional arrays **rowSet** and **columnSet** of length **m** and **n** respectively. We will iterate through the matrix and

```
if matrix[i][j] == 0 then
   rowSet[i] = 1
   columnSet[j] = 1
```

We can then set **matrix[i][j]** to zero if **rowSet[i]** is equal to 1 or **columenSet[j]** is equal to 1

## **Program**

Here is the program for the same.

```
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

**Output**

```
[[0 1 1] [0 0 0] [0 1 1]]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*