<!--yml
category: 未分类
date: 2024-10-13 06:49:06
-->

# Find all Arithmetic series of length greater than two in Go (Golang)

> 来源：[https://golangbyexample.com/arithmetic-series-golang/](https://golangbyexample.com/arithmetic-series-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An Arithmetic series is a series in which the difference between each element is equal. In this program, an array of integers is given. The objective is to find all arithmetic series greater than two.

The problem is best understood with an example

Examples

```
Input: [2,3,4,5]
Output: 3
```

We have three arithmetic slices of length greater than 2 in the above array

*   2,3,4

*   3,4,5

*   2,3,4,5

It is a dynamic programming question as it has an optimal substructure. Let’s say the name of the array is input

*   dp[0] = 0

*   dp[1] = 0

*   dp[2] = 1 if dp[2] – dp[1] == dp[1] – dp[0]

*   dp[i] = 1 if dp[i] – dp[i-1] == dp[i-1] – dp[i-2]

where **dp[i]** represents the number of arithmetic series of length greater than 2 till length **i+1**

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func numberOfArithmeticSlices(nums []int) int {
	lenNums := len(nums)

	if lenNums <= 2 {
		return 0
	}

	dp := make([]int, lenNums)

	dp[0] = 0

	if (nums[2] - nums[1]) == nums[1]-nums[0] {
		dp[2] = 1
	}

	for i := 3; i < lenNums; i++ {
		if nums[i]-nums[i-1] == nums[i-1]-nums[i-2] {
			dp[i] = dp[i-1] + 1

		}
	}

	output := 0
	for i := 2; i < lenNums; i++ {
		output = output + dp[i]
	}

	return output

}

func main() {
	output := numberOfArithmeticSlices([]int{2, 3, 4, 5})
	fmt.Println(output)
}
```

**Output**

```
3
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)Arithmetic series of length greater than two

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*