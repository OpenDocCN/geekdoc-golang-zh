<!--yml
category: 未分类
date: 2024-10-13 06:47:20
-->

# Program to calculate the power of a number in Go (Golang)

> 来源：[https://golangbyexample.com/power-number-golang/](https://golangbyexample.com/power-number-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to calculate the power of a given integer. There will be two inputs

*   The number itself – The number can be positive as well as negative. It can be float as well

*   The power – The power also can be positive as well as negative

Examples

```
Input: Num:2, Power:4
Output: 16

Input: Num:2, Power:-4
Output: 0.0625
```

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func pow(x float64, n int) float64 {

	if x == 0 {
		return 0
	}

	if n == 0 {
		return 1
	}

	if n == 1 {
		return x
	}

	if n == -1 {
		return 1 / x
	}

	val := pow(x, n/2)

	m := x
	if n < 0 {
		m = 1 / x
	}

	if n%2 == 1 || n%2 == -1 {
		return val * val * m
	} else {
		return val * val
	}

}

func main() {
	output := pow(2, 4)
	fmt.Println(output)

	output = pow(2, -4)
	fmt.Println(output)
}
```

**Output**

```
16
0.0625
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*