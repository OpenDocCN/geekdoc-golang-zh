<!--yml
category: 未分类
date: 2024-10-13 06:48:51
-->

# Program to multiply two strings in Go (Golang)

> 来源：[https://golangbyexample.com/multiply-two-strings-golang/](https://golangbyexample.com/multiply-two-strings-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Write a program to multiply two strings.

Examples

```
Input: "12"*"12"
Output: 144

Input: "123"*"12"
Output: 1476
```

## **Program**

Here is the program for the same.

```
package main

import (
	"fmt"
	"math"
	"strconv"
)

func multiply(num1 string, num2 string) string {

	if len(num1) > len(num2) {
		num2, num1 = num1, num2
	}

	output := 0

	k := 0

	carry := 0

	for i := len(num1) - 1; i >= 0; i-- {

		x := 0
		temp := 0
		for j := len(num2) - 1; j >= 0; j-- {

			digit1, _ := strconv.Atoi(string(num1[i]))
			digit2, _ := strconv.Atoi(string(num2[j]))

			multiply_output := digit1*digit2 + carry

			carry = multiply_output / 10

			temp = multiply_output%10*int(math.Pow(10, float64(x))) + temp
			x = x + 1
		}

		temp = carry*int(math.Pow(10, float64(x))) + temp
		carry = 0

		output = temp*int(math.Pow(10, float64(k))) + output
		k = k + 1
	}

	return strconv.Itoa(output)
}

func main() {
	output := multiply("12", "12")
	fmt.Println(output)

	output = multiply("123", "12")
	fmt.Println(output)
}
```

**Output**

```
144
1476
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*