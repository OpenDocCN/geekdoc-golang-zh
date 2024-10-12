<!--yml
category: 未分类
date: 2024-10-13 06:45:31
-->

# Jump Game Program in Go (Golang)

> 来源：[https://golangbyexample.com/jump-game-program-golang/](https://golangbyexample.com/jump-game-program-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## Overview

There is an input array provided. Each entry in the array represents the maximum jump length from that position. One is supposed to start from the first index and return true if the last index can be reached and return false if the last index cannot be reached

Example

```
Input: [2, 3, 1, 1, 4]
Output: true

Input: [3, 2, 1, 0, 4]
Output: false
```

In the first example, there are different ways to reach the last index.

*   0-1-4

*   0-2-3-4

In the second example, there is no way to reach the last index. The best you can reach is the second last index. Since the value at the second last index is zero, therefore it is not possible to reach the last index.

There is another variation where you have to return the minimum number of jumps. We will look at the program later

## **Program**

```
package main

import "fmt"

func canJump(nums []int) bool {
	lenNums := len(nums)
	canJumpB := make([]bool, lenNums)

	canJumpB[0] = true

	for i := 0; i < lenNums; i++ {

		if canJumpB[i] {
			valAtCurrIndex := nums[i]
			for k := 1; k <= valAtCurrIndex && i+k < lenNums; k++ {
				canJumpB[i+k] = true
			}
		}

	}

	return canJumpB[lenNums-1]
}

func main() {
	input := []int{2, 3, 1, 1, 4}

	canJumpOrNot := canJump(input)
	fmt.Println(canJumpOrNot)

	input = []int{3, 2, 1, 0, 4}

	canJumpOrNot = canJump(input)
	fmt.Println(canJumpOrNot)

}
```

**Output**

```
true
false
```

There is another variation where you have to return the minimum number of jumps. Here is the program for the same

```
package main

import "fmt"

func jump(nums []int) int {

	lenJump := len(nums)
	minJumps := make([]int, lenJump)
	for i := 0; i < lenJump; i++ {
		minJumps[i] = -1
	}

	minJumps[0] = 0

	for i := 0; i < lenJump; i++ {
		currVal := nums[i]

		for j := 1; j <= currVal && i+j < lenJump; j++ {
			if minJumps[i+j] == -1 || minJumps[i+j] > minJumps[i]+1 {
				minJumps[i+j] = minJumps[i] + 1
			}
		}
	}

	return minJumps[lenJump-1]

}

func main() {
	input := []int{2, 3, 1, 1, 4}

	minJump := jump(input)
	fmt.Println(minJump)

	input = []int{3, 2, 1, 0, 4}

	minJump = jump(input)
	fmt.Println(minJump)

}
```

**Output**

```
2
-1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*