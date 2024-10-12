<!--yml
category: 未分类
date: 2024-10-13 06:48:04
-->

# Remove all occurrences of a given value in an array in place in Go (Golang)

> 来源：[https://golangbyexample.com/remove-element-golang/](https://golangbyexample.com/remove-element-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An integer array is given and a target element is given. Remove all occurrences of that target element from the array. The removal must be done in place

```
Input: [1, 4, 2, 5, 4]
Target: 4
Output: [1, 2, 5]

Input: [1, 2, 3]
Target:3
Output: [1, 2]
```

## **Program**

Here is the program for the same.

```
package main

import (
	"fmt"
)

func removeElement(nums []int, val int) []int {
	lenNums := len(nums)

	k := 0

	for i := 0; i < lenNums; {
		if nums[i] != val {
			nums[k] = nums[i]
			k++
		}
		i++
	}
	return nums[0:k]
}

func main() {
	output := removeElement([]int{1, 4, 2, 5, 4}, 4)
	fmt.Println(output)

	output = removeElement([]int{1, 2, 3}, 3)
	fmt.Println(output)
}
```

**Output**

```
[1 2 5]
[1 2]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*