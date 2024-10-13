<!--yml

category: 未分类

日期：2024-10-13 06:47:20

-->

# 用 Go（Golang）编写的计算一个数字的幂的程序。

> 来源：[`golangbyexample.com/power-number-golang/`](https://golangbyexample.com/power-number-golang/)

目录

**   概述

+   程序*  *## **概述**

目标是计算给定整数的幂。将有两个输入。

+   数字本身 – 这个数字可以是正数也可以是负数，也可以是浮点数。

+   幂 – 幂可以是正数，也可以是负数。

示例

```go
Input: Num:2, Power:4
Output: 16

Input: Num:2, Power:-4
Output: 0.0625
```

## **程序**

这是相同程序的代码。

```go
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

**输出**

```go
16
0.0625
```

**注意：** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试用实例覆盖所有概念。本教程适合那些希望获得专业知识和对 Golang 有深刻理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

如果你对了解所有设计模式在 Golang 中的实现感兴趣，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
