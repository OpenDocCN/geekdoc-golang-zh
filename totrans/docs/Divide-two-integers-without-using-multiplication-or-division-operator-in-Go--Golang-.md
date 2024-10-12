<!--yml
category: 未分类
date: 2024-10-13 06:46:18
-->

# Divide two integers without using multiplication or division operator in Go (Golang)

> 来源：[https://golangbyexample.com/divide-two-int-golang/](https://golangbyexample.com/divide-two-int-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Two numbers are given. The objective is to divide those numbers and return the quotient. Ignore the remainder in the solution. But division needs to be done without using the multiplication or division operator.

*   The first number is the dividend

*   The second number is the divisor

For example

```
Input: 15,2
Ouput: 7

Input: -15,2
Ouput: -7

Input: 15,-2
Ouput: -7

Input: -15,-2
Ouput: 7
```

Here is the idea of how to do it. The first thing to notice is that

*   If dividend and divisor are either both positive or both negative then the quotient is positive

*   If either one of dividend and divisor is negative then the quotient is negative

So there is an XOR relation between signs of dividend and divisor. We can follow the below steps to write the program

*   First, determine the sign of the quotient as per the XOR logic above.

*   Then make both dividend and divisor positive.

*   Now increase the divisor by itself until it is less than or equal to the dividend. Also, keep a counter for each increment

*   counter*sign will be the answer

## **Program**

Here is the program for the same.

```
package main

import (
	"fmt"
	"math"
)

func divide(dividend int, divisor int) int {

	sign := 1
	if dividend < 0 || divisor < 0 {
		sign = -1
	}

	if dividend < 0 && divisor < 0 {
		sign = 1
	}

	if dividend < 0 {
		dividend = -1 * dividend
	}

	if divisor < 0 {
		divisor = -1 * divisor
	}

	start := divisor

	i := 0

	for start <= dividend {
		start = start + divisor
		i++
	}

	output := i * sign

	return output
}

func main() {
	output := divide(15, 2)
	fmt.Println(output)

	output = divide(-15, 2)
	fmt.Println(output)

	output = divide(15, -2)
	fmt.Println(output)

	output = divide(-15, -2)
	fmt.Println(output)
}
```

**Output**

```
7
-7
-7
7
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*