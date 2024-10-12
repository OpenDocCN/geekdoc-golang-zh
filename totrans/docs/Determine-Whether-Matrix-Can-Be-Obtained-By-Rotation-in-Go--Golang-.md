<!--yml
category: 未分类
date: 2024-10-13 06:50:47
-->

# Determine Whether Matrix Can Be Obtained By Rotation in Go (Golang)

> 来源：[https://golangbyexample.com/matrix-rotation-target-golang/](https://golangbyexample.com/matrix-rotation-target-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Two n*n matrices are given **source** and **target.** We have to determine whether the **source** matrix can be converted to the **target** matrix by doing 90-degree rotations any number of times

**Example 1**

```
Input: source = [2,1],[1,2]], target = [[1,2],[2,1]]
Output: true
```

The source matrix can be rotated 90 degrees once to obtain the target matrix

**Example 2**

```
Input:  source = [[1,2],[2,2]], target = [[2,1],[1,2]]
Output: false
```

Even if we rotate the source matrix 3 times by 90, obtaining the target matrix is impossible.

# **Program**

Below is the program for the same

```
package main

import "fmt"

func findRotation(mat [][]int, target [][]int) bool {
	n, tmp := len(mat), make([]bool, 4)
	for i := range tmp {
		tmp[i] = true
	}
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if mat[i][j] != target[i][j] {
				tmp[0] = false
			}
			if mat[i][j] != target[j][n-i-1] {
				tmp[1] = false
			}
			if mat[i][j] != target[n-i-1][n-j-1] {
				tmp[2] = false
			}
			if mat[i][j] != target[n-j-1][i] {
				tmp[3] = false
			}
		}
	}
	return tmp[0] || tmp[1] || tmp[2] || tmp[3]
}

func main() {
	output := findRotation([][]int{{2, 1}, {1, 2}}, [][]int{{1, 2}, {2, 1}})
	fmt.Println(output)

	output = findRotation([][]int{{1, 2}, {2, 2}}, [][]int{{2, 1}, {1, 2}})
	fmt.Println(output)

}
```

**Output**

```
true
false
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*