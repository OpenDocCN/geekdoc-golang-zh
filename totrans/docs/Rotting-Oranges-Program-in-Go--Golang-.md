<!--yml
category: 未分类
date: 2024-10-13 06:50:36
-->

# Rotting Oranges Program in Go (Golang)

> 来源：[https://golangbyexample.com/rotting-oranges-program-go/](https://golangbyexample.com/rotting-oranges-program-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

An m*n matrix is given where each entry contains three values

*   0 – Denotes that entry is empty

*   1 – Denotes that entry contains fresh orange

*   2 – Denotes that the entry contains rotten orange

A rotten orange will rot the neighboring orange in 1 day. For a given orange, any orange which lies on top, bottom, left and right is a neighboring orange. Diagonal oranges are not counted

The objective is to find the number of days when all the oranges will be rotten. Written -1 if all oranges cannot be rotten. This will happen if a fresh orange is unreachable from a rotten orange

**Example 1**

```
Input: [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

There is one rotten orange at the top. It will rotten its neighboring two oranges. Those rotten neighboring orange will further rotten their neighboring oranges.

**Example 2**

```
Input: [[1,1]]
Output: -1
```

# **Program**

Below is the program for the same

```
package main

import "fmt"

func orangesRotting(grid [][]int) int {
	numRows := len(grid)
	numColumns := len(grid[0])

	queue := make([][2]int, 0)

	for i := 0; i < numRows; i++ {
		for j := 0; j < numColumns; j++ {
			if grid[i][j] == 2 {
				queue = append(queue, [2]int{i, j})
			}
		}
	}

	neighboursIndex := [][2]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
	numMinutes := 0
	for {
		n := len(queue)
		for i := 0; i < n; i++ {
			pop := queue[0]
			queue = queue[1:]

			a := pop[0]
			b := pop[1]
			for k := 0; k < 4; k++ {
				neighbourX := a + neighboursIndex[k][0]
				neighbourY := b + neighboursIndex[k][1]

				if isValid(neighbourX, neighbourY, numRows, numColumns) {
					if grid[neighbourX][neighbourY] == 1 {
						grid[neighbourX][neighbourY] = 2
						queue = append(queue, [2]int{neighbourX, neighbourY})
					}
				}

			}
		}

		if len(queue) == 0 {
			break
		}
		numMinutes++
	}

	for i := 0; i < numRows; i++ {
		for j := 0; j < numColumns; j++ {
			if grid[i][j] == 1 {
				return -1
			}
		}
	}

	return numMinutes
}

func isValid(i, j, numRows, numColumns int) bool {
	if i >= numRows || i < 0 {
		return false
	}

	if j >= numColumns || j < 0 {
		return false
	}
	return true
}

func main() {
	output := orangesRotting([][]int{{2, 1, 1}, {1, 1, 0}, {0, 1, 1}})
	fmt.Println(output)

	output = orangesRotting([][]int{{1, 1}})
	fmt.Println(output)
}
```

**Output:**

```
4
-1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*