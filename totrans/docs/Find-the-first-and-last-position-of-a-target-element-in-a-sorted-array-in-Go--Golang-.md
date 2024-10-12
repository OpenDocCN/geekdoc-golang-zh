<!--yml
category: 未分类
date: 2024-10-13 06:44:00
-->

# Find the first and last position of a target element in a sorted array in Go (Golang)

> 来源：[https://golangbyexample.com/first-last-position-target-sorted-array-golang/](https://golangbyexample.com/first-last-position-target-sorted-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the first and last position of an element in a sorted array.

Eg

```
Input: [1, 2, 2, 5]
Target: 2
Output: [1, 2]

Input: [1, 2, 5]
Target: 2
Output: [1, 1]

Input: []
Target: 1
Output: [-1,-1]
```

Strategy is to

*   Do a binary search in the array and find the left index.

*   Then again do a binary search in the array and find the right index.

## **Program**

Below is the program for the same

```
package main

import "fmt"

func main() {
	output := searchRange([]int{1, 2, 2, 5}, 2)
	fmt.Println(output)

	output = searchRange([]int{1, 2, 5}, 2)
	fmt.Println(output)

	output = searchRange([]int{}, 1)
	fmt.Println(output)
}

func searchRange(nums []int, target int) []int {

	output := make([]int, 2)
	output[0] = findLeftPivot(nums, 0, len(nums)-1, target, len(nums))

	output[1] = findRightPivot(nums, 0, len(nums)-1, target, len(nums))

	return output
}

func findLeftPivot(nums []int, start, end, target, len int) int {
	if start > end {
		return -1
	}

	if start == end && nums[start] == target {
		return start
	}

	mid := (start + end) / 2

	if (mid == 0 || nums[mid-1] < nums[mid]) && nums[mid] == target {
		return mid
	}

	if target <= nums[mid] {
		return findLeftPivot(nums, start, mid-1, target, len)
	}

	return findLeftPivot(nums, mid+1, end, target, len)

}

func findRightPivot(nums []int, start, end, target, len int) int {
	if start > end {
		return -1
	}

	if start == end && nums[start] == target {
		return start

	}

	mid := (start + end) / 2

	if mid+1 <= end && nums[mid] == target && nums[mid] < nums[mid+1] {
		return mid
	}

	if (mid == len-1 || nums[mid] < nums[mid+1]) && nums[mid] == target {
		return mid - 1
	}

	if target >= nums[mid] {
		return findRightPivot(nums, mid+1, end, target, len)
	}

	return findRightPivot(nums, start, mid-1, target, len)

}
```

**Output**

```
[1 2]
[1 1]
[-1 -1]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*