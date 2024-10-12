<!--yml
category: 未分类
date: 2024-10-13 06:44:38
-->

# Sort an array of 0, 1, and 2 in Go (Golang)

> 来源：[https://golangbyexample.com/sort-array-one-zero-two-golang/](https://golangbyexample.com/sort-array-one-zero-two-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [First Approach – Using count](#First_Approach_-_Using_count "First Approach – Using count")
*   [Second Approach - Using switch and shuffling](#Second_Approach_-_Using_switch_and_shuffling "Second Approach - Using switch and shuffling")*  *## **Overview**

The objective is to sort an array of 0,1 and 2 with all 0’s at the start, all 1’s in the middle, and all 2’s in the end. The Space Complexity of the solution should be O(1) which means that you should not allocate any extra space in terms of the input size of the given array

Eg

```
Input: [0, 1, 2, 0, 1]
Output: [0, 0, 1, 1, 2]

Input: [1, 0]
Output: [0, 1]
```

There are two ways of solving this problem

**First Approach:** We can iterate over the array and can keep the count of 0’s, 1’s, and 2’s. Once we have the count, iterate again over the array and assign that number of 0’s at first then the number of 1’s, and then the number of 2’s.

**Second approach:** In the second approach we can have three indexes

*   low initialized to 0

*   mid initialized to 0

*   high initialized to length of input array minus one

Now iterate over the array for the mid position

*   If we encounter a 0 we move it to the left at position **low**. Increment **low** and **mid**

*   If we encounter a 1 we simply do **mid++**

*   If we encounter a 2 we simply move it to right at position **high**. Decrement **high**

Here are the programs for the same

## **First Approach – Using count**

```
package main

import "fmt"

func main() {
	sortNums([]int{2, 0, 2, 1, 1, 0})
}

func sortNums(nums []int) {
	zeroCount := 0
	oneCount := 0
	twoCount := 0

	for i := 0; i < len(nums); i++ {
		switch nums[i] {
		case 0:
			zeroCount++
		case 1:
			oneCount++
		case 2:
			twoCount++
		}
	}
	counter := 0
	for i := 0; i < zeroCount; i++ {
		nums[counter] = 0
		counter++
	}
	for i := 0; i < oneCount; i++ {
		nums[counter] = 1
		counter++
	}
	for i := 0; i < twoCount; i++ {
		nums[counter] = 2
		counter++
	}
	fmt.Println(nums)
}
```

**Output**

```
[0 0 1 1 2 2]
```

## **Second Approach - Using switch and shuffling**

```
package main

import "fmt"

func main() {
	sortNums([]int{2, 0, 2, 1, 1, 0})
}

func sortNums(nums []int) {
	low := 0
	mid := 0
	high := len(nums) - 1

	for mid <= high {

		switch nums[mid] {
		case 0:
			nums[low], nums[mid] = nums[mid], nums[low]
			low++
			mid++
		case 1:
			mid++
		case 2:
			nums[mid], nums[high] = nums[high], nums[mid]
			high--
		}
	}
	fmt.Println(nums)
}
```

**Output**

```
[0 0 1 1 2 2]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*