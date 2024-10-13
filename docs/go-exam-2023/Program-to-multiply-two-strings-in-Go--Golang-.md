<!--yml

分类：未分类

日期：2024-10-13 06:48:51

-->

# 用 Go（Golang）乘两个字符串的程序

> 来源：[`golangbyexample.com/multiply-two-strings-golang/`](https://golangbyexample.com/multiply-two-strings-golang/)

目录

+   概述

+   程序 

## **概述**

编写一个程序来乘两个字符串。

示例

```go
Input: "12"*"12"
Output: 144

Input: "123"*"12"
Output: 1476
```

## **程序**

这是相应的程序。

```go
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

**输出**

```go
144
1476
```

**注意：** 查看我们的 Golang 高级教程。该系列的教程内容详尽，我们尝试用实例覆盖所有概念。本教程适合那些希望获得专业知识和对 Golang 有深入理解的学习者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
