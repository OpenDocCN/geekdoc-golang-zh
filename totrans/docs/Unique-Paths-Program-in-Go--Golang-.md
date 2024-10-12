<!--yml
category: 未分类
date: 2024-10-13 06:48:09
-->

# Unique Paths Program in Go (Golang)

> 来源：[https://golangbyexample.com/unique-paths-program-golang/](https://golangbyexample.com/unique-paths-program-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [First Variation](#First_Variation "First Variation")
    *   [Program](#Program "Program")
    *   [Program](#Program-2 "Program")*  *## **Overview**

There is an m*n grid. There is a robot at the position (0,0). The robot can only move in the right direction and down direction. What is the total number of ways for a robot to reach the right-down corner i.e (m-1, n-1)

Example

```
Input: m=2 , n=2
Output: 2

Robot can reach the right down corner in two ways. 
1\. [0,0] -> [0,1]-> [1, 1]
2\. [0,0] -> [1,0]-> [1, 1]
```

There is another variation of this program where one of the items in the grid could contain an obstacle. Let’s look at the first variation and later we will look at the second variation

## **First Variation**

We will solve this question through dynamic programming

*   Create a paths matrix of size m*n

*   **paths[i][j]** represents the number of ways for robot to reach the (i,j) index

*   **paths[0][0]** = 0

*   **paths[i][j]** = **paths[i-1][j]** + **paths[i][j-1]**

### **Program**

Here is the program for the same.

```
package main

import "fmt"

func uniquePaths(m int, n int) int {
	paths := make([][]int, m)

	for i := 0; i < m; i++ {
		paths[i] = make([]int, n)
	}

	paths[0][0] = 1

	for i := 1; i < m; i++ {
		paths[i][0] = 1
	}

	for i := 1; i < n; i++ {
		paths[0][i] = 1
	}

	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			paths[i][j] = paths[i-1][j] + paths[i][j-1]
		}
	}

	return paths[m-1][n-1]
}

func main() {
	output := uniquePaths(3, 7)
	fmt.Println(output)
}
```

**Output**

```
6
```

**Second Variation**We will also solve this question through dynamic programming

*   Create a paths matrix of size m*n

*   **paths[i][j]** represents the number of ways for robot to reach the (i,j) index

*   **paths[0][0]** = 0

*   If **paths[i][j]** is not an obstacle then **paths[i][j]** = **paths[i-1][j]** + **paths[i][j-1]**

*   If **paths[i][j]** is an obstacle then **paths[i][j]** = 0

### **Program**

```
package main

import "fmt"

func uniquePathsWithObstacles(obstacleGrid [][]int) int {

	m := len(obstacleGrid)
	n := len(obstacleGrid[0])
	paths := make([][]int, len(obstacleGrid))

	for i := 0; i < m; i++ {
		paths[i] = make([]int, n)
	}

	if obstacleGrid[0][0] != 1 {
		paths[0][0] = 1
	}

	for i := 1; i < m; i++ {
		if obstacleGrid[i][0] == 1 {
			break
		} else {
			paths[i][0] = paths[i-1][0]
		}

	}

	for i := 1; i < n; i++ {
		if obstacleGrid[0][i] == 1 {
			break
		} else {
			paths[0][i] = paths[0][i-1]
		}

	}

	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			if obstacleGrid[i][j] != 1 {
				paths[i][j] = paths[i-1][j] + paths[i][j-1]
			}

		}
	}

	return paths[m-1][n-1]
}

func main() {
	output := uniquePathsWithObstacles([][]int{{0, 0, 0}, {0, 1, 0}, {0, 0, 0}})
	fmt.Println(output)
}
```

**Output**

```
2
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*