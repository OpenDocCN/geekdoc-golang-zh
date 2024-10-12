<!--yml
category: 未分类
date: 2024-10-13 06:45:46
-->

# Remove duplicates from a sorted array in Go (Golang)

> 来源：[https://golangbyexample.com/remove-duplicates-sorted-array-golang/](https://golangbyexample.com/remove-duplicates-sorted-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to remove duplicates from a sorted array.

**Examples**

```
Input: [1, 1, 1, 2]
Output: [1, 2]

Input: [1, 2, 3, 3]
Output: [1, 2, 3]
```

## **Program**

Below is the program for the same

```
package main

import "fmt"

func main() {
	input := []int{1, 1, 1, 2}
	output := removeDuplicates(input)
	fmt.Println(output)

	input = []int{1, 2, 3, 3}
	output = removeDuplicates(input)
	fmt.Println(output)
}

func removeDuplicates(nums []int) []int {
	lenArray := len(nums)

	k := 0
	for i := 0; i < lenArray; {
		nums[k] = nums[i]
		k++
		for i+1 < lenArray && nums[i] == nums[i+1] {
			i++
		}
		i++
	}

	return nums[0:k]
}
```

**Output**

```
[1 2]
[1 2 3]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*