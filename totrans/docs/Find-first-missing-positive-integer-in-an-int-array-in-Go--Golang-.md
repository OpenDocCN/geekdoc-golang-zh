<!--yml
category: 未分类
date: 2024-10-13 06:43:29
-->

# Find first missing positive integer in an int array in Go (Golang)

> 来源：[https://golangbyexample.com/first-missing-postivie-integer-golang/](https://golangbyexample.com/first-missing-postivie-integer-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the first missing positive integer in an int array.

Eg

```
Input: [3, 2, -2]
Output: 1

Input: [7, 8, 9, 11, 12]
Output: 1

Input: [1, 2, 3]
Output: 4
```

We can make the index of the array negative if the number equal to that index is present in the array. For this problem, zero and negative numbers don’t matter, so we segregate the positive number on one side and zero/negative numbers on the other side.

Then we act only on the positive numbers and follow the below approach.

*   We iterate through the array. For the current element **x**, we make change the value at **arr[x-1]** to negative.

*   In the end, we iterate and return the index which is still positive. If all index is negative then we return

```
total_num_of_positive_int + 1
```

## **Program**

Below is the program for the same

```
package main

import (
	"fmt"
	"math"
)

func main() {
	output := firstMissingPositive([]int{3, 2, -2})
	fmt.Println(output)

	output = firstMissingPositive([]int{-3, -2, -1})
	fmt.Println(output)

	output = firstMissingPositive([]int{7, 8, 9, 11, 12})
	fmt.Println(output)

	output = firstMissingPositive([]int{1, 2, -1})
	fmt.Println(output)

	output = firstMissingPositive([]int{1, 2, 3})
	fmt.Println(output)

	output = firstMissingPositive([]int{1, 1})
	fmt.Println(output)
}

func firstMissingPositive(nums []int) int {

	onlyPositiveNumsArray, k := segregate((nums))

	for i := 0; i < k; i++ {
		value := int(math.Abs(float64(onlyPositiveNumsArray[i])))

		if value > 0 && value <= k {
			if onlyPositiveNumsArray[value-1] > 0 {
				onlyPositiveNumsArray[value-1] = -1 * onlyPositiveNumsArray[value-1]
			}

		}
	}

	for i := 0; i < k; i++ {
		if onlyPositiveNumsArray[i] > 0 {
			return i + 1
		}
	}

	return k + 1
}

func segregate(nums []int) ([]int, int) {

	k := 0

	for i := 0; i < len(nums); i++ {
		if nums[i] > 0 {
			nums[k] = nums[i]
			k++
		}
	}

	return nums[0:k], k

}
```

**Output**

```
1
1
1
3
4
2
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*