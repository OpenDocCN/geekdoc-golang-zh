<!--yml

类别：未分类

日期：2024-10-13 06:46:13

-->

# 在 Go (Golang) 中添加数字的所有数字的程序

> 来源：[`golangbyexample.com/add-all-digits-number-golang/`](https://golangbyexample.com/add-all-digits-number-golang/)

目录

+   概述

+   程序

## **概述**

目标是反复相加一个数字的所有数字，直到结果仅为一个单一数字。

例如

```go
Input: 453
Step 1: 4+5+3 = 12
Step 2: 1+2 =3

Output: 3
```

另一个示例

```go
Input: 45
Step 1: 4+5 = 9

Output: 9
```

## **程序**

这是相同程序的代码

```go
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

**输出**

```go
3
9
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力覆盖所有概念及示例。此教程适合那些希望获得专业知识和对 Golang 有深入理解的读者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，那么这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


