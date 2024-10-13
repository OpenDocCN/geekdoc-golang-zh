<!--yml

category: 未分类

date: 2024-10-13 06:44:33

-->

# 在 Go 中合并重叠区间（Golang）

> 来源：[`golangbyexample.com/merge-overlapping-intervals-golang/`](https://golangbyexample.com/merge-overlapping-intervals-golang/)

目录

**   概述

+   程序*  *## **概述**

给定一个区间数组，每个区间都有开始时间和结束时间，合并重叠区间。如果第一个区间的结束时间大于第二个区间的开始时间，则这两个区间被称为重叠。假设两个区间都是按开始时间排序的。

示例。

假设我们有下面两个区间

```go
[2,6]
[5,8]
```

然后这些区间是重叠的，因为第一个区间的结束时间大于第二个区间的开始时间。同时，上述区间是按开始时间排序的。

类似地，下面两个区间不重叠

```go
[2,6]
[8,9]
```

因此目标是合并给定数组中的重叠区间

例如。

```go
Input: [[1,4], [8,10], [9,12], [3,5]]
Output: [[1,5], [8,12]]
```

## **程序**

以下将是逻辑

+   根据开始时间对区间数组进行排序。

+   从索引 0 开始合并重叠区间。如上所述，如果第一个区间的结束时间大于第二个区间的开始时间，则这两个区间被称为重叠。

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	output := merge([][]int{{1, 4}, {8, 10}, {9, 12}, {3, 5}})
	fmt.Println(output)

	output = merge([][]int{{1, 4}, {4, 5}})
	fmt.Println(output)

	output = merge([][]int{{2, 2}, {2, 2}})
	fmt.Println(output)

	output = merge([][]int{{2, 3}, {4, 5}, {6, 7}, {8, 9}, {1, 10}})
	fmt.Println(output)
}

type intervalsArray [][]int

func (intA intervalsArray) Len() int {
	return len(intA)
}

func (intA intervalsArray) Swap(i, j int) {
	intA[i], intA[j] = intA[j], intA[i]
}

func (intA intervalsArray) Less(i, j int) bool {
	return intA[i][0] < intA[j][0]
}

func merge(intervals [][]int) [][]int {

	intA := intervalsArray(intervals)

	sort.Sort(intA)

	intervalsSorted := [][]int(intA)
	//fmt.Println(intervalsSorted)

	var output [][]int
	currentIntervalStart := intervalsSorted[0][0]
	currentIntervalEnd := intervalsSorted[0][1]
	for j := 1; j < len(intervalsSorted); j++ {
		if currentIntervalEnd >= intervalsSorted[j][0] {
			if intervalsSorted[j][1] > currentIntervalEnd {
				currentIntervalEnd = intervalsSorted[j][1]
			}
		} else {
			output = append(output, []int{currentIntervalStart, currentIntervalEnd})
			currentIntervalStart = intervalsSorted[j][0]
			currentIntervalEnd = intervalsSorted[j][1]
		}
	}
	output = append(output, []int{currentIntervalStart, currentIntervalEnd})
	return output

}
```

**输出**

```go
[[1 5] [8 12]]
[[1 5]]
[[2 2]]
[[1 10]]
```

**注意：** 查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试覆盖所有概念并附有例子。此教程适合那些希望获得专业知识和扎实理解 Golang 的人——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
