<!--yml
category: 未分类
date: 2024-10-13 06:43:45
-->

# Find the pivot index in a sorted and pivoted array in Go (Golang)

> 来源：[https://golangbyexample.com/pivot-index-pivoted-array-go/](https://golangbyexample.com/pivot-index-pivoted-array-go/)

Table of Contents

 **   [Overview](#Overview "Overview")*  *## **Overview**

We have an input array that is sorted but is pivoted at a certain index. For example, consider below array

```
[1, 3, 5, 7, 9]
```

It has been rotated and pivoted at index 3

```
[5, 7, 9, 1, 3]
```

The objective is to find the pivot index. So the answer for the above array will be 3\. If the array is not pivoted then it should return -1\. For eg for below input array, it should return -1

```
[0, 1, 2, 3, 4]
```

Other examples

```
Pivoted Array: [7, 9, 1, 3, 5]
Pivot Index: 2

Pivoted Array: [9, 1, 3, 5, 9]
Pivot Index: 2

Pivoted Array: [1, 3, 5, 7, 9]
Pivot Index: -1
```

The expected time complexity to find the pivot index is **O(logn)**. So we have to do a binary search with some modifications to find the pivot index.

This will be the strategy

*   Do a binary search. For every mid element check if **mid** or **mid+1** is the pivot

*   If the value at **mid** is less than the value at the start of the input array then search on the left side of **mid**

*   If the value at mid is greater than the value at the start of the input array then search on the right side of **mid**

Below is the program for the same.

```
package main

import "fmt"

func main() {
	pivot := findPivot([]int{0, 1, 2, 3, 4, 5})
	fmt.Println(pivot)

	pivot = findPivot([]int{1, 2, 3, 4, 5, 0})
	fmt.Println(pivot)

	pivot = findPivot([]int{2, 3, 4, 5, 0, 1})
	fmt.Println(pivot)

	pivot = findPivot([]int{3, 4, 5, 0, 1, 2})
	fmt.Println(pivot)

	pivot = findPivot([]int{4, 5, 0, 1, 2, 3})
	fmt.Println(pivot)
	pivot = findPivot([]int{5, 0, 1, 2, 3, 4})
	fmt.Println(pivot)
}

func findPivot(nums []int) int {
	return findPivotUtil(nums, 0, len(nums)-1)
}

func findPivotUtil(nums []int, start, end int) int {
	if start > end {
		return -1
	}

	mid := (start + end) / 2

	if mid+1 <= end && nums[mid] > nums[mid+1] {
		return mid + 1
	}

	if mid-1 >= start && nums[mid] < nums[mid-1] {
		return mid
	}

	if nums[mid] < nums[start] {
		return findPivotUtil(nums, start, mid-1)
	}

	return findPivotUtil(nums, mid+1, end)

}

func binarySearch(nums []int, start, end, target int) int {
	if start > end {
		return -1
	}

	mid := (start + end) / 2

	if nums[mid] == target {
		return mid
	}

	if target < nums[mid] {
		return binarySearch(nums, start, mid-1, target)
	} else {
		return binarySearch(nums, mid+1, end, target)
	}

}
```

**Output**

```
-1
5
4
3
2
1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*