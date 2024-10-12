<!--yml
category: 未分类
date: 2024-10-13 06:43:50
-->

# Search in a sorted and pivoted array in Go (Golang)

> 来源：[https://golangbyexample.com/search-sorted-pivoted-array/](https://golangbyexample.com/search-sorted-pivoted-array/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

We have an input array that is sorted but is pivoted at a certain index. For example, consider below array

```
[1, 3, 5, 7, 9]
```

It has been rotated and pivoted at index 3

```
[5, 7, 9, 1, 3]
```

Given a target element, the objective is to find the index of a target element in that sorted and pivoted array in **O(logn)** time. It should return -1 if the target element doesn’t exist in the given array

For eg

```
Input: [5, 7, 9, 1, 3]
Target Element: 7
Output: 2

Input: [5, 7, 9, 1, 3]
Target Element: 8
Output: -1
```

Below will be the strategy

*   Find the pivot index in the given input array using binary search with some modifications

*   If the target element is less th**a**n the start element of the array then do a binary search from the pivot end to the end of the array

*   If the target element is greater than the start element of the array then do a binary search from start to pivot-1 index

Below will be the strategy to find the pivot index

*   Do a binary search. For every mid element check if **mid** or **mid+1** is the pivot

*   If the value at **mid** is less than the value at the start of the input array then search on the left side of **mid**

*   If the value at mid is greater than the value at the start of the input array then search on the right side of **mid**

## **Program**

Below is the program for the same.

```
package main

import "fmt"

func main() {
	output := search([]int{4, 5, 6, 7, 0, 1, 2}, 0)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 1)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 2)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 4)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 5)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 6)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 7)
	fmt.Println(output)

	output = search([]int{4, 5, 6, 7, 0, 1, 2}, 3)
	fmt.Println(output)

	output = search([]int{1, 2}, 3)
	fmt.Println(output)
}

func search(nums []int, target int) int {
	pivot := findPivot(nums)

	if pivot == -1 {
		return binarySearch(nums, 0, len(nums)-1, target)
	}

	if target < nums[0] {
		return binarySearch(nums, pivot, len(nums)-1, target)
	}

	return binarySearch(nums, 0, pivot-1, target)
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
4
5
6
0
1
2
3
-1
-1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*