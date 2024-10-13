<!--yml

类别：未分类

日期：2024-10-13 06:49:38

-->

# Go（Golang）中的非重叠区间程序

> 来源：[https://golangbyexample.com/non-overlapping-intervals-golang/](https://golangbyexample.com/non-overlapping-intervals-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

给定一个区间数组，其中intervals[i] = [starti, endi]。我们需要找出最少需要移除的区间数量，以使区间数组中的区间不重叠。

让我们通过一个例子来理解。

```go
Input: intervals = [[2,3],[3,4],[4,5],[2,4]]
Output: 1
Explanation: [2,4] can be removed and the rest of the intervals are non-overlapping.
```

思路是先按区间开始时间排序，然后计算重叠的区间。

## **程序**

这里是相应的程序。

```go
package main

import (
	"fmt"
	"sort"
)

func eraseOverlapIntervals(intervals [][]int) int {
	lenIntervals := len(intervals)

	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})

	prevIntervalEnd := intervals[0][1]

	minIntervals := 0
	for i := 1; i < lenIntervals; i++ {
		currentIntervalStart := intervals[i][0]
		currentIntervalEnd := intervals[i][1]

		if currentIntervalStart < prevIntervalEnd {
			minIntervals++
			if prevIntervalEnd >= currentIntervalEnd {
				prevIntervalEnd = currentIntervalEnd
			}
		} else {
			prevIntervalEnd = currentIntervalEnd
		}
	}
	return minIntervals
}

func main() {

	output := eraseOverlapIntervals([][]int{{2, 3}, {3, 4}, {4, 5}, {2, 4}})
	fmt.Println(output)
}
```

**输出**

```go
6
13
```

**注意：** 查看我们的Golang高级教程。该系列的教程内容详尽，我们尝试覆盖所有概念并提供示例。此教程适合希望获得Golang专业知识和扎实理解的人——[Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，那么这篇文章适合你——[Golang中的所有设计模式](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
