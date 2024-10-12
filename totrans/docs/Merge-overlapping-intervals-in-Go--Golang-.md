<!--yml
category: 未分类
date: 2024-10-13 06:44:33
-->

# Merge overlapping intervals in Go (Golang)

> 来源：[https://golangbyexample.com/merge-overlapping-intervals-golang/](https://golangbyexample.com/merge-overlapping-intervals-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Given an array of intervals where each interval has a start time and end time, merge the overlapping intervals. Two intervals are said to be overlapping if the end time of the first interval is greater than the start time of the second interval. This is assuming that both the intervals are sorted on start time.

Example.

Let’s say we have below two intervals

```
[2,6]
[5,8]
```

Then these intervals are overlapping as the end time of the first interval is greater than the start time of the second interval.  Also here the above intervals are sorted on start time.

Similarly, below two intervals are not overlapping

```
[2,6]
[8,9]
```

So the objective is to merge overlapping intervals in a given array of intervals

Eg.

```
Input: [[1,4], [8,10], [9,12], [3,5]]
Output: [[1,5], [8,12]]
```

## **Program**

Below will be the logic

*   Sort the interval array on the basis of the start time.

*   Start from index 0 and merge overlapping intervals. As mentioned above two intervals are said to be overlapping if the end time of the first interval is greater than the start time of the second interval

```
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

**Output**

```
[[1 5] [8 12]]
[[1 5]]
[[2 2]]
[[1 10]]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*