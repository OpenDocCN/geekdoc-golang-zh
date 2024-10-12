<!--yml
category: 未分类
date: 2024-10-13 06:46:13
-->

# Program to add all digits of a number in Go (Golang)

> 来源：[https://golangbyexample.com/add-all-digits-number-golang/](https://golangbyexample.com/add-all-digits-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to repeatedly add all digits of a number until the result is only a single digit.

For example

```
Input: 453
Step 1: 4+5+3 = 12
Step 2: 1+2 =3

Output: 3
```

Another example

```
Input: 45
Step 1: 4+5 = 9

Output: 9
```

## **Program**

Here is the program for the same

```
package main

import "fmt"

func addDigits(num int) int {

	if num < 10 {
		return num
	}

	for num > 9 {
		num = sum(num)
	}

	return num

}

func sum(num int) int {
	output := 0
	for num > 0 {
		output = output + num%10
		num = num / 10
	}
	return output
}

func main() {
	output := addDigits(453)
	fmt.Println(output)

	output = addDigits(45)
	fmt.Println(output)

}
```

**Output**

```
3
9
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*