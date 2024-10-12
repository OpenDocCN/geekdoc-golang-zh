<!--yml
category: 未分类
date: 2024-10-13 06:50:25
-->

# Find the number which appears once in an array in Go(Golang)

> 来源：[https://golangbyexample.com/number-array-once-golang/](https://golangbyexample.com/number-array-once-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

An array is given in which every element is present twice except one element. The objective is to find that element in constant extra space

**Example 1**

```
Input: [2, 1, 2, 3, 3]
Output: 1
```

**Example 2**

```
Input: [1, 1, 4]
Output: 4
```

The idea is to use XOR here. Here we will use two properties of XOR

*   XOR of a number with itself is 0

*   XOR of 0 and any number is that number

So the idea is to do an XOR of all the numbers in the array. The number that we obtain at the end will be the answer.

# **Program**

Below is the program for the same

```
package main

import "fmt"

func singleNumber(nums []int) int {
	lenNums := len(nums)
	res := 0
	for i := 0; i < lenNums; i++ {
		res = res ^ nums[i]
	}

	return res
}

func main() {
	output := singleNumber([]int{2, 1, 2, 3, 3})
	fmt.Println(output)

	output = singleNumber([]int{1, 1, 4})
	fmt.Println(output)

}
```

**Output:**

```
1
4
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)*