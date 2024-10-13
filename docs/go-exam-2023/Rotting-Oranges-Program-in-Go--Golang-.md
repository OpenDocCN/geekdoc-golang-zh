<!--yml

分类：未分类

日期：2024-10-13 06:50:36

-->

# Go语言中的腐烂橙子程序（Golang）

> 来源：[https://golangbyexample.com/rotting-oranges-program-go/](https://golangbyexample.com/rotting-oranges-program-go/)

目录

**   [概述](#Overview "概述")

+   [程序](#Program "程序")*  *# **概述**

给定一个m*n矩阵，其中每个条目包含三个值

+   0 – 表示该条目为空

+   1 – 表示该条目包含新鲜橙子

+   2 – 表示该条目包含腐烂的橙子

腐烂的橙子将在1天内腐烂相邻的橙子。对于给定的橙子，位于上、下、左和右的任何橙子都是相邻橙子。对角线的橙子不算在内。

目标是找出所有橙子腐烂的天数。如果所有橙子无法腐烂，则写-1。这种情况发生在新鲜橙子无法从腐烂橙子到达时。

**示例 1**

```go
Input: [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

顶部有一个腐烂的橙子。它将腐烂相邻的两个橙子。这些腐烂的橙子将进一步腐烂它们相邻的橙子。

**示例 2**

```go
Input: [[1,1]]
Output: -1
```

# **程序**

下面是相应的程序

```go
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

**输出：**

```go
4
-1
```

**注意：**查看我们的Golang高级教程。本系列的教程内容详尽，我们试图用示例覆盖所有概念。这个教程是为那些希望获得专业知识和对Golang有扎实理解的人准备的 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

同时，查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
