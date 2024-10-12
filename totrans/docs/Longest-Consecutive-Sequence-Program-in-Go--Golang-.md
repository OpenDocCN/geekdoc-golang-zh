<!--yml
category: 未分类
date: 2024-10-13 06:51:02
-->

# Longest Consecutive Sequence Program in Go (Golang)

> 来源：[https://golangbyexample.com/longest-consecutive-sequence-golang/](https://golangbyexample.com/longest-consecutive-sequence-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

An integer array is given. Find the length of the longest consecutive sequence in it. Let’s see an example to understand it

**Example 1**

```
Input: [4,7,3,8,2,1]
Output: 4
Reason: The longest consecutive sequence is [1,2,3,4]
```

**Example 2**

```
Input: [4,7,3,8,2,1,9,24,10,11]
Output: 5
Reason: The longest consecutive sequence is [7,8,9,10,11]
```

The naive idea is to sort the array and return the longest consecutive sequence. But we can do this in O(n) time. Idea is to use a hash here. Below is the idea

*   Store each of the numbers in the hash.

*   Then starting with each number, find the length of the longest consecutive sequence beginning at that number. So if a number let’s say is n. We try to find n+1, n+2… in the hash and get the length of the longest consecutive sequence  starting from that number

*   If the length find in step 2 is greater than the previous longest consecutive sequence found, then update the longest consecutive sequence length

# **Program**

Below is the program for the same

```
package main

import "fmt"

func longestConsecutive(nums []int) int {
	numsMap := make(map[int]int)

	lenNums := len(nums)

	for i := 0; i < lenNums; i++ {
		numsMap[nums[i]] = 0
	}

	maxLCS := 0
	for i := 0; i < lenNums; i++ {
		currentLen := 1
		counter := 1

		for {
			val, ok := numsMap[nums[i]+counter]
			if ok {
				if val != 0 {
					currentLen += val
					break
				} else {
					currentLen += 1
					counter += 1
				}
			} else {
				break
			}
		}

		if currentLen > maxLCS {
			maxLCS = currentLen
		}
		numsMap[nums[i]] = currentLen
	}

	return maxLCS
}

func main() {
	output := longestConsecutive([]int{4, 7, 3, 8, 2, 1})
	fmt.Println(output)

	output = longestConsecutive([]int{4, 7, 3, 8, 2, 1, 9, 24, 10, 11})
	fmt.Println(output)

}
```

**Output:**

```
4
5
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*