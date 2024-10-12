<!--yml
category: 未分类
date: 2024-10-13 06:42:54
-->

# Find the next permutation of a number in Go (Golang)

> 来源：[https://golangbyexample.com/next-permuation-number-go/](https://golangbyexample.com/next-permuation-number-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the next permutation of a given number based upon lexicographical sorting order. If the next permutation is not possible then return the same number

For eg

```
Input: [1,2,3]
Output: [1,3,2]

Input: [2,1]
Output: [2,1]

Input: [1, 3, 5, 4, 1]
Output: [1, 4, 1, 3, 5]

Input: [1, 3, 2]
Output: [2, 1, 3]
```

Below will be the strategy

*   Start from the left and find the first number which is less than the number on its right. Let’s say that number is found at index **“first”**

*   Then find the smallest greater than the number on the index **first** on the right part of the array after the **first** index. Then replace that number with the number on the index **first**

*   Sort the right part of the array after the index **first**

## **Program**

Below is the program for the same

```
package main

import (
	"fmt"
	"sort"
)

func main() {
	nextPermutation([]int{1, 2, 3})

	nextPermutation([]int{2, 1})

	nextPermutation([]int{1, 3, 5, 4, 1})

	nextPermutation([]int{1, 3, 2})
}

func nextPermutation(nums []int) {

	numsLen := len(nums)
	first := -1
	second := -1

	for i, j := numsLen-2, numsLen-1; i >= 0; {
		if nums[i] < nums[j] {
			first = i
			second = j
			break
		} else {
			i--
			j--
		}
	}

	if !(first == -1) {
		smallestGreaterIndex := second
		for i := second + 1; i < numsLen; i++ {
			if nums[i] > nums[first] && nums[i] < nums[smallestGreaterIndex] {
				smallestGreaterIndex = i
			}
		}
		nums[first], nums[smallestGreaterIndex] = nums[smallestGreaterIndex], nums[first]

		sort.Slice(nums[second:numsLen], func(i, j int) bool {
			return nums[second+i] < nums[second+j]
		})
	}

	fmt.Println(nums)

}
```

**Output**

```
[1 3 2]
[2 1]
[1 4 1 3 5]
[2 1 3]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*