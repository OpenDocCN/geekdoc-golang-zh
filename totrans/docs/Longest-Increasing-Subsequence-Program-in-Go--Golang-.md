<!--yml
category: 未分类
date: 2024-10-13 06:46:28
-->

# Longest Increasing Subsequence Program in Go (Golang)

> 来源：[https://golangbyexample.com/longest-increasing-subsequence-golang/](https://golangbyexample.com/longest-increasing-subsequence-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the longest increasing subsequence in a given input array. It is the longest subsequence in a given sequence such that every element is larger than its previous element

For example

```
Input: [1,5,7,6]
The longest subsequence is [1,5,6] which is of length 3
Output: 3
```

Another example

```
Input: [3,2,1]
The longest subsequence is either {3}, {2} or {1}. Each is of length 1
Output: 1
```

Longest Increasing Subsequence is a DP problem. Assume the input array is named **input** only. Let’s say **lis** is the array where **lis[i]** is the length of the longest increasing subsequence at index **i**.

Then

*   **lis[0]** = 1

*   **lis[i]** = **max(lis[j])** + 1 where 0 <= **j** < **i** and **input[i]** > **input[j]**

*   **lis[i]** = 1 if there is no such **j**

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func lengthOfLIS(nums []int) int {

	lenNums := len(nums)
	lis := make([]int, lenNums)

	for i := 0; i < lenNums; i++ {
		lis[i] = 1
	}

	for i := 1; i < lenNums; i++ {
		for j := 0; j < i; j++ {
			if nums[i] > nums[j] && lis[i] < (lis[j]+1) {
				lis[i] = lis[j] + 1
			}
		}
	}

	max := 0

	for i := 0; i < lenNums; i++ {
		if lis[i] > max {
			max = lis[i]
		}
	}

	return max
}

func main() {
	output := lengthOfLIS([]int{1, 5, 7, 6})
	fmt.Println(output)

	output = lengthOfLIS([]int{3, 2, 1})
	fmt.Println(output)
}
```

**Output**

```
3
1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*