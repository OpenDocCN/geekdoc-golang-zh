<!--yml
category: 未分类
date: 2024-10-13 06:48:24
-->

# Plus one program or Add one to an integer array in Go (Golang)

> 来源：[https://golangbyexample.com/add-one-integer-array-go/](https://golangbyexample.com/add-one-integer-array-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An integer array is given. Overall this integer array represents a number. So let’s say the integer array name is digits then digitis[i] denote the ith digit of the integer. The objective is to add 1 to that to that integer array. This has to be done without converting that array to a number of type int.

Example

```
Input: [1, 2]
Output: [1, 3]

Input: [9, 9]
Output: [1, 0, 0]
```

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func plusOne(digits []int) []int {

	lenDigits := len(digits)

	output := make([]int, 0)

	lastDigit := digits[lenDigits-1]

	add := lastDigit + 1
	carry := add / 10

	lastDigit = add % 10
	output = append(output, lastDigit)

	for i := lenDigits - 2; i >= 0; i-- {
		o := digits[i] + carry
		lastDigit = o % 10
		carry = o / 10
		output = append(output, lastDigit)
	}

	if carry == 1 {
		output = append(output, 1)
	}

	return reverse(output, len(output))
}

func reverse(input []int, length int) []int {
	start := 0
	end := length - 1

	for start < end {
		input[start], input[end] = input[end], input[start]
		start++
		end--
	}

	return input
}

func main() {
	output := plusOne([]int{1, 2})
	fmt.Println(output)

	output = plusOne([]int{9, 9})
	fmt.Println(output)
}
```

**Output**

```
[1 3]
[1 0 0]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*