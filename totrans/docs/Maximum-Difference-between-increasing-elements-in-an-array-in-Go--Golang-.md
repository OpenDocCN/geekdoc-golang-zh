<!--yml
category: 未分类
date: 2024-10-13 06:49:48
-->

# Maximum Difference between increasing elements in an array in Go (Golang)

> 来源：[https://golangbyexample.com/maximum-difference-increasing-elements-in-an-array-golang/](https://golangbyexample.com/maximum-difference-increasing-elements-in-an-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An array is given. The objective is to find the maximum difference between values at two indexes i and j such that

*   j > i
*   arr[j] > arr[i]

If no such indexes exist then return -1

Example 1

```
Input: intervals = [8, 2, 6, 5]
Output: 4
Explanation: 6-2 = 4
```

Example 2

```
Input: intervals = [8, 3, 2, 1]
Output: -1
Explanation: Condition is not satified
```

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func maximumDifference(nums []int) int {
	lenNums := len(nums)
	if lenNums == 0 || lenNums == 1 {
		return -1
	}
	minElement := nums[0]

	maxDifference := -1

	for i := 1; i < lenNums; i++ {
		diff := nums[i] - minElement
		if diff > maxDifference && diff != 0 {
			maxDifference = diff
		}

		if nums[i] < minElement {
			minElement = nums[i]
		}
	}

	return maxDifference
}

func main() {
	output := maximumDifference([]int{8, 2, 6, 5})
	fmt.Println(output)

	output = maximumDifference([]int{8, 3, 2, 1})
	fmt.Println(output)

}
```

**Output**

```
4
-1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*