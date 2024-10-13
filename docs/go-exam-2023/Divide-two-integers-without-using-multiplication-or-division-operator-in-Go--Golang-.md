<!--yml

类别：未分类

日期：2024-10-13 06:46:18

-->

# 在 Go (Golang) 中不使用乘法或除法运算符来除以两个整数。

> 来源：[`golangbyexample.com/divide-two-int-golang/`](https://golangbyexample.com/divide-two-int-golang/)

目录

**   概述

+   程序*  *## **概述**

给定两个数字。目标是将这两个数字相除并返回商。在解决方案中忽略余数。但需要在不使用乘法或除法运算符的情况下进行除法。

+   第一个数字是被除数。

+   第二个数字是除数。

例如

```go
Input: 15,2
Ouput: 7

Input: -15,2
Ouput: -7

Input: 15,-2
Ouput: -7

Input: -15,-2
Ouput: 7
```

这里是如何实现它的思路。首先要注意的是

+   如果被除数和除数都是正数或都是负数，则商为正。

+   如果被除数和除数其中一个为负，则商为负。

所以被除数和除数的符号之间存在异或关系。我们可以遵循以下步骤来编写程序。

+   首先，根据上述异或逻辑确定商的符号。

+   然后将被除数和除数都变为正数。

+   现在，持续增加除数，直到它小于或等于被除数。同时，保持每次递增的计数器。

+   计数器*符号将是答案。

## **程序**

这里是相同程序的代码。

```go
package main

import (
	"fmt"
	"math"
)

func divide(dividend int, divisor int) int {

	sign := 1
	if dividend < 0 || divisor < 0 {
		sign = -1
	}

	if dividend < 0 && divisor < 0 {
		sign = 1
	}

	if dividend < 0 {
		dividend = -1 * dividend
	}

	if divisor < 0 {
		divisor = -1 * divisor
	}

	start := divisor

	i := 0

	for start <= dividend {
		start = start + divisor
		i++
	}

	output := i * sign

	return output
}

func main() {
	output := divide(15, 2)
	fmt.Println(output)

	output = divide(-15, 2)
	fmt.Println(output)

	output = divide(15, -2)
	fmt.Println(output)

	output = divide(-15, -2)
	fmt.Println(output)
}
```

**输出**

```go
7
-7
-7
7
```

**注意：** 查看我们的 Golang 高级教程。本系列的教程详尽，我们试图用例子覆盖所有概念。本教程适合那些希望获得高水平理解和扎实知识的学习者 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
