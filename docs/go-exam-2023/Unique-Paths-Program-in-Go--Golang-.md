<!--yml

分类：未分类

日期：2024-10-13 06:48:09

-->

# Golang中的唯一路径程序

> 来源：[https://golangbyexample.com/unique-paths-program-golang/](https://golangbyexample.com/unique-paths-program-golang/)

目录

**   [概述](#Overview "Overview")**

+   [第一种变体](#First_Variation "First Variation")

    +   [程序](#Program "Program")

    +   [程序](#Program-2 "Program")*  *## **概述**

有一个m*n的网格。在位置(0,0)有一个机器人。机器人只能向右和向下移动。机器人到达右下角即(m-1, n-1)的总方式数量是多少？

示例

```go
Input: m=2 , n=2
Output: 2

Robot can reach the right down corner in two ways. 
1\. [0,0] -> [0,1]-> [1, 1]
2\. [0,0] -> [1,0]-> [1, 1]
```

这个程序还有另一种变体，其中网格中的一个项可能包含障碍物。我们先看看第一种变体，稍后再看第二种变体。

## **第一种变体**

我们将通过动态编程解决这个问题。

+   创建一个大小为m*n的路径矩阵。

+   **paths[i][j]**表示机器人到达(i,j)索引的方式数量。

+   **paths[0][0]** = 0。

+   **paths[i][j]** = **paths[i-1][j]** + **paths[i][j-1]**。

### **程序**

这是相同程序的代码。

```go
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

**输出**

```go
6
```

**第二种变体**我们将通过动态编程解决这个问题。

+   创建一个大小为m*n的路径矩阵。

+   **paths[i][j]**表示机器人到达(i,j)索引的方式数量。

+   **paths[0][0]** = 0。

+   如果**paths[i][j]**不是障碍物，则**paths[i][j]** = **paths[i-1][j]** + **paths[i][j-1]**。

+   如果**paths[i][j]**是障碍物，则**paths[i][j]** = 0。

### **程序**

```go
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

**输出**

```go
2
```

**注意：** 请查看我们的Golang高级教程。本系列的教程内容详尽，我们尝试用示例覆盖所有概念。本教程适合那些希望获得专业知识并深入理解Golang的读者 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章适合你 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
