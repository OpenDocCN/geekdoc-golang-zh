<!--yml
category: 未分类
date: 2024-10-13 06:49:11
-->

# The nth digit in a sequence program in Go (Golang)

> 来源：[https://golangbyexample.com/nth-digit-sequence-golang/](https://golangbyexample.com/nth-digit-sequence-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Given an integer n find the nth digit in the infinite sequence {1, 2, 3, 4 ….. infinity}

Example

```
Input: 14
Output: 1
```

The 14th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 … is a 1, which is part of the number 12.

Example 2

```
Input: 17
Output: 3
```

The 17th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13 … is a 3, which is part of the number 13.

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func findNthDigit(n int) int {

	numDigits := 1
	tenIncrement := 1

	start := 1
	counter := 9 * tenIncrement * numDigits

	for n > counter {
		n = n - counter
		tenIncrement = tenIncrement * 10
		numDigits++
		start = start * 10
		counter = 9 * tenIncrement * numDigits
	}

	return findNthDigitUtil(start, numDigits, n)

}

func findNthDigitUtil(start, numDigits, n int) int {
	position := n % numDigits
	digitWhichHasN := 0
	if position == 0 {
		digitWhichHasN = start - 1 + n/numDigits
		return digitWhichHasN % 10
	} else {
		digitWhichHasN = start + n/numDigits
		positionFromBehind := numDigits - position
		answer := 0
		for positionFromBehind >= 0 {
			answer = digitWhichHasN % 10
			digitWhichHasN = digitWhichHasN / 10
			positionFromBehind--
		}
		return answer
	}

	return 0

}

func main() {
	output := findNthDigit(14)
	fmt.Println(output)

	output = findNthDigit(17)
	fmt.Println(output)
} 
```

**Output**

```
1
3
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*