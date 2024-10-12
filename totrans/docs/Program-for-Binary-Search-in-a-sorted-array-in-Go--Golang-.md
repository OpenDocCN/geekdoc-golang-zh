<!--yml
category: 未分类
date: 2024-10-13 06:50:14
-->

# Program for Binary Search in a sorted array in Go (Golang)

> 来源：[https://golangbyexample.com/binary-search-sorted-array-golang/](https://golangbyexample.com/binary-search-sorted-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Idea is to do a binary search of a given target element in an input array. If the target element is present then output the index. If the output element is not present then output -1\.

Expected Time Complexity if O(logn)

**Example 1**

```
Input: [1, 4, 5, 6]
Target Element: 4
Output: 1

Target element 4 is present at index 1
```

**Example 2**

```
Input: [1, 2, 3]
Target Element: 4
Output: -1

Target element 4 is present at index 1
```

## **Program**

```
package main

import "fmt"

func search(nums []int, target int) int {
	return binarySearch(nums, 0, len(nums)-1, target)
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

func main() {
	index := search([]int{1, 4, 5, 6}, 4)
	fmt.Println(index)

	index = search([]int{1, 2, 3, 6}, 4)
	fmt.Println(index)
}
```

**Output**

```
1
-1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*