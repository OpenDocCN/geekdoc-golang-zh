<!--yml
category: 未分类
date: 2024-10-13 06:50:20
-->

# Majority element in an array in Go (Golang)

> 来源：[https://golangbyexample.com/majority-element-array-golang/](https://golangbyexample.com/majority-element-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

The objective is to find the majority element in a given array. A majority element is an element that occurs more than n/2 times in a given array where n is the length of the array

**Example 1**

```
Input: [2, 1, 2, 2, 3]
Output: 2
```

**Example 2**

```
Input: [1]
Output: 1
```

# **Program**

Below is the program for the same

```
package main

import "fmt"

func majorityElement(nums []int) int {
	lenNums := len(nums)

	if lenNums == 1 {
		return nums[0]
	}

	numsMap := make(map[int]int)

	for i := 0; i < lenNums; i++ {
		_, ok := numsMap[nums[i]]
		if ok {
			numsMap[nums[i]] = numsMap[nums[i]] + 1
			if numsMap[nums[i]] > lenNums/2 {
				return nums[i]
			}
		} else {
			numsMap[nums[i]] = 1
		}
	}

	return 0

}

func main() {
	output := majorityElement([]int{2, 1, 2, 2, 3})
	fmt.Println(output)

	output = majorityElement([]int{1})
	fmt.Println(output)
}
```

**Output:**

```
2
1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*