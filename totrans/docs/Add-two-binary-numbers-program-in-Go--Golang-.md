<!--yml
category: 未分类
date: 2024-10-13 06:45:36
-->

# Add two binary numbers program in Go (Golang)

> 来源：[https://golangbyexample.com/add-two-binary-numbers-golang/](https://golangbyexample.com/add-two-binary-numbers-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to add two given binary numbers. Binary numbers are only composed of digits 0 and 1\. Below is the binary addition logic for individual digits

*   0+0 = Sum is 0, Carry is 0

*   0+1 = Sum is 1, Carry is 0

*   1+0 = Sum is 0, Carry is 0

*   1+1 = Sum is 0, Carry is 1

*   1+1+1 = Sum is 1, Carry is 1

Examples

```
Input: "101" + "11"
Output: "1000"

Input: "111" + "101"
Output: "1100"
```

## **Program**

Below is the program for the same

```
package main

import (
	"fmt"
	"strconv"
)

func addBinary(a string, b string) string {
	lenA := len(a)
	lenB := len(b)

	i := lenA - 1
	j := lenB - 1

	var output string
	var sum int
	carry := 0
	for i >= 0 && j >= 0 {
		first := int(a[i] - '0')
		second := int(b[j] - '0')

		sum, carry = binarySum(first, second, carry)

		output = strconv.Itoa(sum) + output
		i = i - 1
		j = j - 1
	}

	for i >= 0 {
		first := int(a[i] - '0')

		sum, carry = binarySum(first, 0, carry)

		output = strconv.Itoa(sum) + output
		i = i - 1

	}

	for j >= 0 {
		second := int(b[j] - '0')

		sum, carry = binarySum(0, second, carry)

		output = strconv.Itoa(sum) + output
		j = j - 1
	}

	if carry > 0 {
		output = strconv.Itoa(1) + output
	}

	return output
}

func binarySum(a, b, carry int) (int, int) {
	output := a + b + carry

	if output == 0 {
		return 0, 0
	}

	if output == 1 {
		return 1, 0
	}

	if output == 2 {
		return 0, 1
	}

	if output == 3 {
		return 1, 1
	}

	return 0, 0
}

func main() {
	output := addBinary("101", "11")
	fmt.Println(output)

	output = addBinary("111", "101")
	fmt.Println(output)
}
```

**Output**

```
1000
1100
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*