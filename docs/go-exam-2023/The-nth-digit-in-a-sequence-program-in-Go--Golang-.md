<!--yml

类别：未分类

日期：2024-10-13 06:49:11

-->

# Go（Golang）中的第 n 位数字序列程序

> 来源：[`golangbyexample.com/nth-digit-sequence-golang/`](https://golangbyexample.com/nth-digit-sequence-golang/)

目录

+   概述

+   程序

## **概述**

给定一个整数 n，找出无限序列 {1, 2, 3, 4 ….. 无限} 中的第 n 位数字。

示例

```go
Input: 14
Output: 1
```

序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 … 的第 14 位数字是 1，它是数字 12 的一部分。

示例 2

```go
Input: 17
Output: 3
```

序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13 … 的第 17 位数字是 3，它是数字 13 的一部分。

## **程序**

这里是相同的程序。

```go
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

**输出**

```go
1
3
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们努力涵盖所有概念及示例。本教程适合那些希望获得专业知识并深入理解 Golang 的人 – [Golang 综合教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
