<!--yml

分类：未分类

日期：2024-10-13 06:48:24

-->

# 加一程序或在Go语言中给整数数组加一

> 来源：[https://golangbyexample.com/add-one-integer-array-go/](https://golangbyexample.com/add-one-integer-array-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

给定一个整数数组。总体来说，这个整数数组表示一个数字。那么假设这个整数数组的名字是digits，digits[i]表示这个整数的第i位。目标是将这个整数数组加1。必须在不将数组转换为int类型数字的情况下完成这个任务。

示例

```go
Input: [1, 2]
Output: [1, 3]

Input: [9, 9]
Output: [1, 0, 0]
```

## **程序**

这里是相应的程序。

```go
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

**输出**

```go
[1 3]
[1 0 0]
```

**注意：** 请查看我们的Golang高级教程。本系列教程内容详尽，我们努力涵盖所有概念及示例。本教程适合希望获得Golang专业知识和扎实理解的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [Go语言](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*
