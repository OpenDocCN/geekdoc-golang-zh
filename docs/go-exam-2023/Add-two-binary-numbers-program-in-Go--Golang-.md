<!--yml

category: 未分类

date: 2024-10-13 06:45:36

-->

# 在 Go (Golang) 中添加两个二进制数的程序

> 来源：[`golangbyexample.com/add-two-binary-numbers-golang/`](https://golangbyexample.com/add-two-binary-numbers-golang/)

目录

**   概述

+   程序*  *## **概述**

目标是将两个给定的二进制数相加。二进制数仅由数字 0 和 1 组成。下面是单个数字的二进制加法逻辑

+   0+0 = 和是 0，进位是 0

+   0+1 = 和是 1，进位是 0

+   1+0 = 和是 0，进位是 0

+   1+1 = 和是 0，进位是 1

+   1+1+1 = 和是 1，进位是 1

示例

```go
Input: "101" + "11"
Output: "1000"

Input: "111" + "101"
Output: "1100"
```

## **程序**

以下是相同程序

```go
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

**输出**

```go
1000
1100
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们尝试涵盖所有概念及示例。本教程适合那些希望获得 Golang 专业知识和扎实理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对如何在 Golang 中实现所有设计模式感兴趣，那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
