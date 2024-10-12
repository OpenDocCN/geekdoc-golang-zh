<!--yml
category: 未分类
date: 2024-10-13 06:48:45
-->

# Search Insert Position Program in Go (Golang)

> 来源：[https://golangbyexample.com/search-insert-position-golang/](https://golangbyexample.com/search-insert-position-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

A sorted input array having distinct integers and a target value are given. The objective is to find the insert position for that target value in that array. There are two cases

*   If the target value exists in the given array, then return that index.

*   If the target value doesn’t exist in the given array, return the position at which it will be inserted

Examples

```
Input: [1,2,3]
Target Value: 4
Output: 3

Input: [1,2,3]
Target Value: 3
Output: 2
```

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func searchInsert(nums []int, target int) int {
	lenNums := len(nums)

	index := -1

	if target <= nums[0] {
		return 0
	}

	if target > nums[lenNums-1] {
		return lenNums
	}

	for i := 0; i < lenNums; i++ {
		if target <= nums[i] {
			index = i
			break
		}
	}

	return index

}

func main() {
	pos := searchInsert([]int{1, 2, 3}, 4)
	fmt.Println(pos)

	pos = searchInsert([]int{1, 2, 3}, 3)
	fmt.Println(pos)
}
```

**Output**

```
3
2
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*