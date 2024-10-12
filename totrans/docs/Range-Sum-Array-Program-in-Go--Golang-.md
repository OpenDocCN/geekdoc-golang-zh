<!--yml
category: 未分类
date: 2024-10-13 06:47:08
-->

# Range Sum Array Program in Go (Golang)

> 来源：[https://golangbyexample.com/range-sum-array-golang/](https://golangbyexample.com/range-sum-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

There is an array of numbers that are given. The objective is to find the range sum in that given array. What does it mean that a range will be given which will have a left index and a right index. And we have to find the sum between the left index and the right index in the given array of numbers.

Seem simple right. Just iterate from the left index to the right index in the given array and return the sum. But here is the catch. Allowed TC is O(1)

Here is the approach we can follow so that we are able to return the answer in O(1) time complexity

*   Pre compute another sum_array from the given array of numbers

*   sum_array[i] = sum of numbers from the 0 index to the ith  index.

*   For a given range with left index and right index  simply return sum_array[left-1] – sum_array[right]. Of course, we need to validate that left-1 is greater than or equal to zero.

## **Program**

Here is the program for the same.

```
package main

import "fmt"

type NumArray struct {
	sum_nums []int
}

func initNumArray(nums []int) NumArray {
	length := len(nums)
	sum_nums := make([]int, length)

	sum_nums[0] = nums[0]

	for i := 1; i < length; i++ {
		sum_nums[i] = nums[i] + sum_nums[i-1]
	}

	return NumArray{
		sum_nums: sum_nums,
	}
}

func (this *NumArray) SumRange(left int, right int) int {
	leftSum := 0
	if left > 0 {
		leftSum = this.sum_nums[left-1]
	}
	return this.sum_nums[right] - leftSum
}

func main() {
	nums := []int{1, 3, 5, 6, 2}
	na := initNumArray(nums)

	output := na.SumRange(2, 4)
	fmt.Println(output)

}
```

**Output**

```
13
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*