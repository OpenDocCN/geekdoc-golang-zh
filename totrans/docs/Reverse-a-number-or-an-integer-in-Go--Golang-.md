<!--yml
category: 未分类
date: 2024-10-13 06:42:10
-->

# Reverse a number or an integer in Go (Golang)

> 来源：[https://golangbyexample.com/reverse-number-golang/](https://golangbyexample.com/reverse-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to reverse a number that is an integer. Integer can be negative as well. Some examples

```
Input: 123
Output: 321

Input: 140
Output: 41

Input: -123
Output: -321

Input: 0
Output: 0
```

Here is the strategy

*   First, make the input number positive

*   Use the below logic to iterate over the number and get the last digit every time. Use the last digit to create the **reversedDigit**

```
for x > 0 {
	lastDigit := x % 10
	reversedDigit = reversedDigit*10 + lastDigit

	x = x / 10
}
```

Here is the complete program for the same.

## **Program**

```
package main

import (
	"fmt"
	"math"
)

func main() {
	reversedInteger := reverse(123)
	fmt.Println(reversedInteger)

	reversedInteger = reverse(140)
	fmt.Println(reversedInteger)

	reversedInteger = reverse(-123)
	fmt.Println(reversedInteger)

	reversedInteger = reverse(0)
	fmt.Println(reversedInteger)
}

func reverse(x int) int {
	sign := "positive"
	if x >= 0 {
		sign = "positive"
	} else {
		sign = "negative"
	}

	x = int(math.Abs(float64(x)))

	var reversedDigit int

	for x > 0 {
		lastDigit := x % 10
		reversedDigit = reversedDigit*10 + lastDigit

		x = x / 10
	}

	if sign == "negative" {
		reversedDigit = reversedDigit * -1
	}
	return reversedDigit
}
```

**Output**

```
321
41
-321
0
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*