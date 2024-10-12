<!--yml
category: 未分类
date: 2024-10-13 06:49:25
-->

# Distinct or Unique Permutations of a string or array in Go (Golang)

> 来源：[https://golangbyexample.com/unique-permutations-golang/](https://golangbyexample.com/unique-permutations-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Given an array of integers that can contain duplicate elements, find all distinct permutations only.

Example

```
Input: [2, 2, 1]
Output: [[2 2 1] [2 1 2] [1 2 2]]

Input: [2, 2, 1, 1]
Output: [[2 2 1 1] [2 1 2 1] [2 1 1 2] [2 1 1 2] [2 1 2 1] [1 2 2 1] [1 2 1 2] [1 1 2 2] [1 2 1 2] [1 2 2 1] [1 1 2 2]]
```

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func permuteUnique(nums []int) [][]int {
	return permuteUtil(nums, 0, len(nums), len(nums))
}

func shouldSwap(nums []int, start, index int) bool {
	for i := start; i < index; i++ {
		if nums[start] == nums[index] {
			return false
		}

	}
	return true

}
func permuteUtil(nums []int, start, end int, length int) [][]int {
	output := make([][]int, 0)
	if start == end-1 {
		return [][]int{nums}
	} else {
		for i := start; i < end; i++ {
			if shouldSwap(nums, start, i) {
				nums[start], nums[i] = nums[i], nums[start]
				n := make([]int, length)
				for k := 0; k < length; k++ {
					n[k] = nums[k]
				}
				o := permuteUtil(n, start+1, end, length)
				output = append(output, o...)
				nums[i], nums[start] = nums[start], nums[i]
			}

		}
	}
	return output
}

func main() {
	output := permuteUnique([]int{2, 2, 1})
	fmt.Println(output)

	output = permuteUnique([]int{2, 2, 1, 1})
	fmt.Println(output)
}
```

**Output**

```
[[2 2 1] [2 1 2] [1 2 2]]
[[2 2 1 1] [2 1 2 1] [2 1 1 2] [2 1 1 2] [2 1 2 1] [1 2 2 1] [1 2 1 2] [1 1 2 2] [1 2 1 2] [1 2 2 1] [1 1 2 2]]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*