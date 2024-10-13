<!--yml

类别：未分类

日期：2024-10-13 06:50:42

-->

# 在 Go (Golang) 中引爆最大炸弹的程序

> 来源：[https://golangbyexample.com/detonate-maximum-bombs-golang/](https://golangbyexample.com/detonate-maximum-bombs-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

给定一个二维数组，其中每个数组的条目有三个值

+   i - 表示炸弹的 x 坐标

+   j - 表示炸弹的 y 坐标

+   r - 表示炸弹范围的半径

一个炸弹爆炸时会导致其范围内所有炸弹的爆炸。当这些炸弹被引爆时，它们又会导致其范围内所有炸弹的爆炸。

你只能引爆一个炸弹。目标是找到可以引爆的最大炸弹数量

**示例 1**

```go
Input: [[1,0,3],[5,0,4]]
Output: 2
```

第一个炸弹位于第二个炸弹的范围内。因此，当我们引爆第二个炸弹时，第二个和第一个炸弹都会被引爆。

**示例 2**

```go
Input:  [[2,2,2],[10,10,5]]
Output: 1
```

两个炸弹彼此不在范围内。

解决这个问题的思路是将一切视为有向图，如果第二个炸弹位于第一个炸弹的范围内，则从第一个炸弹到第二个炸弹存在一个有向节点。

一旦构建了这个图，我们可以从每个节点进行深度优先搜索（DFS），以获取它可以引爆的最大炸弹数量。我们还会存储先前计算的结果。

# **程序**

下面是相应的程序

```go
package main

import (
	"fmt"
	"math"
)

func maximumDetonation(bombs [][]int) int {

	if len(bombs) == 0 {
		return 0
	}
	max := 1
	detonationMap := make(map[int][]int)

	for i := 0; i < len(bombs); i++ {
		for j := 0; j < len(bombs); j++ {
			if i != j {
				if float64(bombs[i][2]) >= distance(bombs[i], bombs[j]) {
					if arr, ok := detonationMap[i]; ok {
						arr = append(arr, j)
						detonationMap[i] = arr
					} else {
						var arr []int
						arr = append(arr, j)
						detonationMap[i] = arr
					}
				}
			}
		}
	}

	for key := range detonationMap {
		detonated := 1
		queue := []int{key}
		visited := make(map[int]bool)
		visited[key] = true

		for len(queue) > 0 {
			cur := queue[0]
			queue = queue[1:]

			for _, val := range detonationMap[cur] {
				if !visited[val] {
					detonated++
					visited[val] = true
					queue = append(queue, val)
				}
			}
		}
		if detonated == len(bombs) {
			return len(bombs)
		}

		if detonated > max {
			max = detonated
		}
	}
	return max
}

func distance(a []int, b []int) float64 {
	ret := math.Sqrt(math.Pow(float64(a[0]-b[0]), 2) + math.Pow(float64(a[1]-b[1]), 2))
	return ret
}

func main() {
	output := maximumDetonation([][]int{{1, 0, 3}, {5, 0, 4}})
	fmt.Println(output)

	output = maximumDetonation([][]int{{2, 2, 2}, {10, 10, 5}})
	fmt.Println(output)

}
```

**输出：**

```go
2
1
```

**注意：** 查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尽力涵盖所有概念并附上示例。本教程适合那些希望获得专业知识和对 Golang 有扎实理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解所有设计模式在 Golang 中如何实现感兴趣。如果是的话，这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

另外，请查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
