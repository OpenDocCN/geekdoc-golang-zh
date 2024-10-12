<!--yml
category: 未分类
date: 2024-10-13 06:49:01
-->

# Find All Duplicates in an Array in Go (Golang)

> 来源：[https://golangbyexample.com/find-all-duplicates-array-golang/](https://golangbyexample.com/find-all-duplicates-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An array is given in which all elements are in the range [1, n] where n is the length of the array. The objective is to find all duplicates in that given array

Examples

```
Input: [1, 2, 3, 2, 4, 3]
Output: [2, 3]
```

The idea here is to take advantage of the fact that numbers are in the range [1, n]. For every element in the array, increase the value at its index by n. So

*   For getting a value at an index we value%n

*   In the end, if the value at any index is greater than 2*n then it is duplicated.

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func findDuplicates(nums []int) []int {
	lenNums := len(nums)

	for i := 0; i < lenNums; i++ {
		index := (nums[i] - 1) % lenNums

		nums[index] = lenNums + nums[index]
	}

	k := 0

	for i := 0; i < lenNums; i++ {
		if nums[i] > 2*lenNums {
			nums[k] = i + 1
			k++
		}
	}

	return nums[0:k]

}

func main() {
	output := findDuplicates([]int{1, 2, 3, 2, 4, 3})
	fmt.Println(output)
}
```

**Output**

```
[2 3]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*