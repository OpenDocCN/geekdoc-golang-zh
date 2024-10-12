<!--yml
category: 未分类
date: 2024-10-13 06:51:07
-->

# Program for Pascal’s Triangle in Go (Golang)

> 来源：[https://golangbyexample.com/program-pascal-triangle-golang/](https://golangbyexample.com/program-pascal-triangle-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")

*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the print n number of rows of Pascal’s triangle. The number n is given as an input to the program

**Example 1**

```
Input: numRows = 4
Output: [[1],[1,1],[1,2,1],[1,3,3,1]]
```

**Example 2**

```
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

Refer to this link to know more about Pascal’s Triangle here – [https://en.wikipedia.org/wiki/Pascal%27s_triangle](https://en.wikipedia.org/wiki/Pascal%27s_triangle)

The idea is to use dynamic programming here.

# **Program**

Below is the program for the same

```
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

**Output:**

```
[1] [1 1] [1 2 1] [1 3 3 1]]
[[1] [1 1] [1 2 1] [1 3 3 1] [1 4 6 4 1]]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*