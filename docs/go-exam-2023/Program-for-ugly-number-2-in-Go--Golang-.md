<!--yml

类别：未分类

日期：2024-10-13 06:51:44

-->

# Go (Golang) 中的丑陋数字 2 的程序

> 来源：[`golangbyexample.com/program-for-ugly-number-2-in-go-golang/`](https://golangbyexample.com/program-for-ugly-number-2-in-go-golang/)

目录

**   概述

+   程序*  *# **概述**

丑陋数字是其质因数限于 2、3 和 5 的数字。我们已经见过一个程序，它给定一个数字 n，返回 true 如果它是丑陋数字，否则返回 false。下面是那个程序的链接 [`golangbyexample.com/ugly-number-golang/`](https://golangbyexample.com/ugly-number-golang/)

在本教程中，我们将编写一个程序，给定一个数字 n，找到第 n 个丑陋数字。

**示例 1**

```go
Input: 5
Output: 5
Reason: First five ugly numbers are 1, 2, 3, 4, 5 hence 5th ugly number is 5
```

**示例 2**

```go
Input: 20
Output: 36
```

这个想法是使用动态编程。我们将跟踪 2、3 和 5 的倍数。下一个丑陋数字将始终是这三个中的最小值。

# **程序**

以下是相同的程序

```go
package main

import "fmt"

func nthUglyNumber(n int) int {
	dpUgly := make([]int, n)

	dpUgly[0] = 1

	next_multiple_of_2 := 2
	next_multiple_of_3 := 3
	next_multiple_of_5 := 5

	i2 := 0
	i3 := 0
	i5 := 0

	for i := 1; i < n; i++ {
		nextUglyNumber := minOfThree(next_multiple_of_2, next_multiple_of_3, next_multiple_of_5)
		dpUgly[i] = nextUglyNumber

		if nextUglyNumber == next_multiple_of_2 {
			i2++
			next_multiple_of_2 = 2 * dpUgly[i2]
		}

		if nextUglyNumber == next_multiple_of_3 {
			i3++
			next_multiple_of_3 = 3 * dpUgly[i3]
		}

		if nextUglyNumber == next_multiple_of_5 {
			i5++
			next_multiple_of_5 = 5 * dpUgly[i5]
		}

	}

	return dpUgly[n-1]

}

func minOfThree(a, b, c int) int {
	if a < b && a < c {
		return a
	}

	if b < c {
		return b
	}

	return c
}

func main() {
	output := nthUglyNumber(5)
	fmt.Println(output)

	output = nthUglyNumber(20)
	fmt.Println(output)
}
```

**输出**

```go
5
36
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，试图覆盖所有概念和示例。本教程适合那些希望获得 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

此外，请查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
