<!--yml

类别：未分类

日期：2024-10-13 06:51:55

-->

# 在 Go (Golang) 中计数无防守单元格的程序

> 来源：[`golangbyexample.com/count-unguarded-cells-golang/`](https://golangbyexample.com/count-unguarded-cells-golang/)

目录

+   概述

+   程序

# **概述**

给定两个整数 m 和 n，表示一个 m*n 网格。除此之外，还给定两个 2D 整数数组

+   守卫的位置为 guards[i] = [rowi , columni]。它表示第 i 个守卫的位置

+   墙壁的位置为 guards[j] = [rowi , columni]。它表示第 i 个墙壁的位置

守卫可以向所有方向看

+   北

+   南

+   东

+   西

除非被墙壁阻挡。守卫能看到的所有单元格都算作已防守。目标是找出无防守单元格的数量

这里是相同的 leetcode 链接 – https://leetcode.com/problems/count-unguarded-cells-in-the-grid/

# **程序**

以下是相同的程序

```go
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

**输出**

```go
7
4
```

**注意：**查看我们的 Golang 高级教程。本系列教程内容详尽，我们尝试覆盖所有概念并附有示例。本教程适合那些希望获得专业知识和对 golang 有深入理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式在 Golang 中如何实现。如果是，那么这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

此外，查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)


