<!--yml
category: 未分类
date: 2024-10-13 06:51:55
-->

# Count unguarded cells program in Go (Golang)

> 来源：[https://golangbyexample.com/count-unguarded-cells-golang/](https://golangbyexample.com/count-unguarded-cells-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Two integers m and n are given which represent an m*n grid. Other than that two 2D integer arrays are also given

*   guards where guards[i] = [rowi , columni]. It represents the position of the ith guard

*   walls where guards[j] = [rowi , columni]. It represents the position of the ith wall

A guard can see in all the directions

*   North

*   South

*   East

*   West

unless obstructed by the wall. All the cells that the guard can see are counted as guarded. The objective is to find the number of unguarded cells

Here is the leetcode link for the same – https://leetcode.com/problems/count-unguarded-cells-in-the-grid/

# **Program**

Below is the program for the same

```
package main

import "fmt"

func countUnguarded(m int, n int, guards [][]int, walls [][]int) int {

	occupancy := make([][]int, m)

	for i := 0; i < m; i++ {
		occupancy[i] = make([]int, n)
	}

	for _, val := range guards {
		i := val[0]
		j := val[1]
		occupancy[i][j] = 2
	}

	for _, val := range walls {
		i := val[0]
		j := val[1]
		occupancy[i][j] = -1
	}

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if occupancy[i][j] == 2 {
				countUtil(i, j, m, n, &occupancy)
			}

		}
	}

	count := 0
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if occupancy[i][j] == 0 {
				count += 1
			}

		}
	}

	return count
}

func countUtil(x, y, m, n int, occupancy *([][]int)) {

	for i := x + 1; i < m; i++ {
		if (*occupancy)[i][y] == 0 {
			(*occupancy)[i][y] = 1
		}

		if (*occupancy)[i][y] == -1 || (*occupancy)[i][y] == 2 {
			break
		}
	}

	for i := x - 1; i >= 0; i-- {
		if (*occupancy)[i][y] == 0 {
			(*occupancy)[i][y] = 1
		}

		if (*occupancy)[i][y] == -1 || (*occupancy)[i][y] == 2 {
			break
		}
	}

	for i := y + 1; i < n; i++ {
		if (*occupancy)[x][i] == 0 {
			(*occupancy)[x][i] = 1
		}

		if (*occupancy)[x][i] == -1 || (*occupancy)[x][i] == 2 {
			break
		}
	}

	for i := y - 1; i >= 0; i-- {

		if (*occupancy)[x][i] == 0 {
			(*occupancy)[x][i] = 1
		}

		if (*occupancy)[x][i] == -1 || (*occupancy)[x][i] == 2 {
			break
		}
	}

}

func main() {
	output := countUnguarded(4, 6, [][]int{{0, 0}, {1, 1}, {2, 3}}, [][]int{{0, 1}, {2, 2}, {1, 4}})
	fmt.Println(output)

	output = countUnguarded(3, 3, [][]int{{1, 1}}, [][]int{{0, 1}, {1, 0}, {2, 1}, {1, 2}})
	fmt.Println(output)
}
```

**Output**

```
7
4
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*