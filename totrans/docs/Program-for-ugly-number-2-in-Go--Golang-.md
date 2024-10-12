<!--yml
category: 未分类
date: 2024-10-13 06:51:44
-->

# Program for ugly number 2 in Go (Golang)

> 来源：[https://golangbyexample.com/program-for-ugly-number-2-in-go-golang/](https://golangbyexample.com/program-for-ugly-number-2-in-go-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

An ugly number is a number whose prime factors are limited to 2,3 and 5\. We have already seen a program where given a number n returns true if it is an ugly number otherwise returns false. Below is the link to that program [https://golangbyexample.com/ugly-number-golang/](https://golangbyexample.com/ugly-number-golang/)

In this tutorial, we will write a program where given a number n, find the nth ugly number

**Example 1**

```
Input: 5
Output: 5
Reason: First five ugly numbers are 1, 2, 3, 4, 5 hence 5th ugly number is 5
```

**Example 2**

```
Input: 20
Output: 36
```

The idea is to use dynamic programming here. We will keep track of multiple of 2, 3, and 5\. The next ugly number will always be the minimum of these three.

# **Program**

Below is the program for the same

```
package main

import "fmt"

func nthUglyNumber(n int) int {
	dpUgly := make([]int, n)

	dpUgly[0] = 1

	next_multiple_of_2 := 2
	next_multiple_of_3 := 3
	next_multiple_of_5 := 5

	i2 := 0
	i3 := 0
	i5 := 0

	for i := 1; i < n; i++ {
		nextUglyNumber := minOfThree(next_multiple_of_2, next_multiple_of_3, next_multiple_of_5)
		dpUgly[i] = nextUglyNumber

		if nextUglyNumber == next_multiple_of_2 {
			i2++
			next_multiple_of_2 = 2 * dpUgly[i2]
		}

		if nextUglyNumber == next_multiple_of_3 {
			i3++
			next_multiple_of_3 = 3 * dpUgly[i3]
		}

		if nextUglyNumber == next_multiple_of_5 {
			i5++
			next_multiple_of_5 = 5 * dpUgly[i5]
		}

	}

	return dpUgly[n-1]

}

func minOfThree(a, b, c int) int {
	if a < b && a < c {
		return a
	}

	if b < c {
		return b
	}

	return c
}

func main() {
	output := nthUglyNumber(5)
	fmt.Println(output)

	output = nthUglyNumber(20)
	fmt.Println(output)
}
```

**Output**

```
5
36
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*