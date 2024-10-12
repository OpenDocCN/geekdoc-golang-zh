<!--yml
category: 未分类
date: 2024-10-13 06:51:39
-->

# Program for ugly number in Go (Golang)

> 来源：[https://golangbyexample.com/ugly-number-golang/](https://golangbyexample.com/ugly-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

An ugly number is a number whose prime factors are limited to 2,3 and 5.

Given a number, n return true if it is an ugly number otherwise return false.

**Example 1**

```
Input: 12
Output: true
```

**Example 2**

```
Input: 7 
Output: false
```

Idea is to

*   While the number is a factor of two keep dividing the number by 2

*   While the number is a factor of three keep dividing the number by 3

*   While the number is a factory of five keep dividing the number by 5

In the end, if the number is equal to 1 then return true otherwise return false

# **Program**

Below is the program for the same

```
package main

import "fmt"

func isUgly(n int) bool {
	if n == 0 {
		return false
	}
	if n == 1 {
		return true
	}

	for {
		if n%2 != 0 {
			break
		}
		n = n / 2
	}

	for {
		if n%3 != 0 {
			break
		}
		n = n / 3
	}

	for {
		if n%5 != 0 {
			break
		}
		n = n / 5
	}

	return n == 1
}

func main() {
	output := isUgly(12)
	fmt.Println(output)

	output = isUgly(7)
	fmt.Println(output)
}
```

**Output**

```
true
false
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)*