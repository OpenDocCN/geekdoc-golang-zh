<!--yml

类别：未分类

日期：2024-10-13 06:50:47

-->

# 确定矩阵是否可以通过旋转在Go (Golang)中获得

> 来源：[https://golangbyexample.com/matrix-rotation-target-golang/](https://golangbyexample.com/matrix-rotation-target-golang/)

目录

**   [概述](#Overview "概述")

+   [程序](#Program "程序")*  *# **概述**

给定两个n*n矩阵，**源**和**目标**。我们需要确定**源**矩阵是否可以通过任意次数的90度旋转转换为**目标**矩阵

**示例 1**

```go
Input: source = [2,1],[1,2]], target = [[1,2],[2,1]]
Output: true
```

源矩阵可以旋转90度一次以获得目标矩阵

**示例 2**

```go
Input:  source = [[1,2],[2,2]], target = [[2,1],[1,2]]
Output: false
```

即使我们将源矩阵旋转3次90度，也无法获得目标矩阵。

# **程序**

下面是相同的程序

```go
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

**输出**

```go
true
false
```

**注意：** 请查看我们的Golang高级教程。本系列教程内容详尽，我们尽力覆盖所有概念并提供示例。本教程适合那些希望获得Golang专业知识和扎实理解的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章就是为你准备的 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

另外，请查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*
