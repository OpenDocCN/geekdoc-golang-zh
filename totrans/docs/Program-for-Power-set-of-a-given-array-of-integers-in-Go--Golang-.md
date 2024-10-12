<!--yml
category: 未分类
date: 2024-10-13 06:47:53
-->

# Program for Power set of a given array of integers in Go (Golang)

> 来源：[https://golangbyexample.com/power-set-array-golang/](https://golangbyexample.com/power-set-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An integer array is given which has all unique elements. The objective is to return the power set of that array

```
Input: [1, 2]
Output: [[],[1],[2],[1,2]]

Input: [1, 2, 3]
Output: [[] [1] [2] [1 2] [3] [1 3] [2 3] [1 2 3]]
```

If the number of elements in the given array is n, then the number of elements in the power set will be pow(2, n). Let’s say n is 3  then the number of elements in the power set will be pow(2, n)=8

Let’s say we take all binary conversion of numbers from 0 to (8-1) ie from 0 to 7.

```
000
001
010
011
100
101
110
111
```

Each of the binary numbers above represents a power set

Eg

```
000 - []
001 - [1]
010 - [2]
011 - [1, 2]
100 - [3]
101 - [1, 3]
110 - [2, 3]
111 - [1, 2, 3]
```

## **Program**

Here is the program for the same.

```
package main

import (
	"fmt"
	"math"
)

func subsets(nums []int) [][]int {

	lengthNums := len(nums)
	powerSetLength := int(math.Pow(2, float64(lengthNums)))
	output := make([][]int, 0)

	for i := 0; i < powerSetLength; i++ {

		result := make([]int, 0)
		for j := 0; j < lengthNums; j++ {
			val := int(i) & int(1<
```

**Output**

```
[[] [1] [2] [1 2]]
[[] [1] [2] [1 2] [3] [1 3] [2 3] [1 2 3]]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)*