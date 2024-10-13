<!--yml

类别：未分类

日期：2024-10-13 06:45:31

-->

# Go 中的跳跃游戏程序

> 来源：[`golangbyexample.com/jump-game-program-golang/`](https://golangbyexample.com/jump-game-program-golang/)

目录

+   概述

+   程序

## 概述

提供了一个输入数组。数组中的每个条目表示从该位置的最大跳跃长度。应该从第一个索引开始，如果可以到达最后一个索引，则返回 true；如果无法到达，则返回 false。

示例

```go
Input: [2, 3, 1, 1, 4]
Output: true

Input: [3, 2, 1, 0, 4]
Output: false
```

在第一个示例中，有不同的方法可以到达最后一个索引。

+   0-1-4

+   0-2-3-4

在第二个示例中，无法到达最后一个索引。你能到达的最远是倒数第二个索引。由于倒数第二个索引的值为零，因此无法到达最后一个索引。

还有一种变体，要求返回最少跳跃次数。我们稍后会讨论这个程序。

## **程序**

```go
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

**输出**

```go
true
false
```

还有一种变体，要求返回最少跳跃次数。下面是该程序

```go
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

**输出**

```go
2
-1
```

**注意：** 查看我们的 Golang 高级教程。该系列教程详细且覆盖了所有概念及示例。本教程适合希望获得专业知识和扎实理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


