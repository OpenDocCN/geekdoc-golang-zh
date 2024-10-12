<!--yml
category: 未分类
date: 2024-10-13 06:49:38
-->

# Non-Overlapping intervals program in Go (Golang)

> 来源：[https://golangbyexample.com/non-overlapping-intervals-golang/](https://golangbyexample.com/non-overlapping-intervals-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An array of intervals is given where intervals[i] = [starti, endi]. We have to find out the minimum number of intervals to remove so that the interval in the intervals array become non-overlapping

Let’s understand it with an example

```
Input: intervals = [[2,3],[3,4],[4,5],[2,4]]
Output: 1
Explanation: [2,4] can be removed and the rest of the intervals are non-overlapping.
```

The idea is to first sort based interval start time and then count the overlapping intervals.

## **Program**

Here is the program for the same.

```
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

**Output**

```
6
13
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*