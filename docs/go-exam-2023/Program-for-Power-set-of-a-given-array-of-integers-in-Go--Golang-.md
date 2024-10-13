<!--yml

分类：未分类

日期：2024-10-13 06:47:53

-->

# 用 Go (Golang)编写的给定整数数组的幂集程序。

> 来源：[`golangbyexample.com/power-set-array-golang/`](https://golangbyexample.com/power-set-array-golang/)

目录

**概述**

+   程序

## **概述**

给定一个包含所有唯一元素的整数数组。目标是返回该数组的幂集。

```go
Input: [1, 2]
Output: [[],[1],[2],[1,2]]

Input: [1, 2, 3]
Output: [[] [1] [2] [1 2] [3] [1 3] [2 3] [1 2 3]]
```

如果给定数组中的元素数量为 n，则幂集中的元素数量将为 pow(2, n)。假设 n 为 3，则幂集中的元素数量为 pow(2, n)=8。

假设我们对数字从 0 到(8-1)进行所有二进制转换，即从 0 到 7。

```go
000
001
010
011
100
101
110
111
```

上面的每个二进制数字表示一个幂集。

例如

```go
000 - []
001 - [1]
010 - [2]
011 - [1, 2]
100 - [3]
101 - [1, 3]
110 - [2, 3]
111 - [1, 2, 3]
```

## **程序**

这是相应的程序。

```go
package main

import (
	"fmt"
	"math"
)

func subsets(nums []int) [][]int {

	lengthNums := len(nums)
	powerSetLength := int(math.Pow(2, float64(lengthNums)))
	output := make([][]int, 0)

	for i := 0; i < powerSetLength; i++ {

		result := make([]int, 0)
		for j := 0; j < lengthNums; j++ {
			val := int(i) & int(1<
```

**输出**

```go
[[] [1] [2] [1 2]]
[[] [1] [2] [1 2] [3] [1 3] [2 3] [1 2 3]]
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程详尽，我们尽力涵盖所有概念和示例。本教程适合那些希望获得专业知识和扎实理解 golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)*
