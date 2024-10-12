<!--yml
category: 未分类
date: 2024-10-13 06:49:53
-->

# Maximum Length of Contiguous Subarray with an equal number of 0’s and 1’s in Go (Golang)

> 来源：[https://golangbyexample.com/max-length-array-zero-one-golang/](https://golangbyexample.com/max-length-array-zero-one-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An array is given that only has 0’s and 1’s. The objective is to find a maximum length subarray with an equal number of 0’s and 1’s. Let’s understand it with examples

Example 1

```
Input: intervals = [0, 1]
Output: 2
```

Example 2

```
Input: intervals = [0, 1, 1, 0, 1, 1]
Output: 4
```

Below is the approach we can take

*   We will create an array **leftSum** where leftSum[i] will denote the sum of numbers from index 0 to i. Treat 0 as -1 and 1 as 1 only.

*   Now there are two cases. The subarray starts from index 0 or the subarray starts from an index other than zero.

*   Scan the leftSum array from left to right. If the value at any index in the **leftSum** is zero, then it means that subarray[0,i] contains an equal number of 0’s and 1’s.  This will give the answer if the subarray starts from index 0

*   If the subarray doesn’t start from zero. So again scan the leftSum array and find index **i** and **j** such that **leftSum[i] == leftSum[j].** To figure it out we will use a map. If the length of j-i is greater than the maximum length, then update the maximum length

*   Return the maximum length in the end

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func findMaxLength(nums []int) int {
	lenNums := len(nums)

	if lenNums == 0 {
		return 0
	}

	currentSum := 0

	sumLeft := make([]int, lenNums)

	for i := 0; i < lenNums; i++ {
		if nums[i] == 0 {
			currentSum = currentSum - 1
		} else {
			currentSum = currentSum + 1
		}
		sumLeft[i] = currentSum
	}

	maxLength := 0

	max := 0
	min := 0

	for i := 0; i < lenNums; i++ {
		if sumLeft[i] == 0 {
			maxLength = i + 1
		}
		if sumLeft[i] > max {
			max = sumLeft[i]
		}

		if sumLeft[i] < min {
			min = sumLeft[i]
		}
	}

	numMap := make(map[int]int, max-min+1)

	for i := 0; i < lenNums; i++ {
		index, ok := numMap[sumLeft[i]]

		if ok {
			currentLength := i - index
			if currentLength > maxLength {
				maxLength = currentLength
			}
		} else {
			numMap[sumLeft[i]] = i
		}
	}

	return maxLength

}
func main() {
	output := findMaxLength([]int{0, 1})
	fmt.Println(output)

	output = findMaxLength([]int{0, 1, 1, 0, 1, 1})
	fmt.Println(output)
}
```

**Output**

```
2
4
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*