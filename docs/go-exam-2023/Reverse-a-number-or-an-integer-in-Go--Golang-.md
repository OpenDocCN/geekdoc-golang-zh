<!--yml

分类：未分类

日期：2024-10-13 06:42:10

-->

# 在Go语言中反转一个数字或整数

> 来源：[https://golangbyexample.com/reverse-number-golang/](https://golangbyexample.com/reverse-number-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是反转一个整数。整数可以是负数。以下是一些示例

```
Input: 123
Output: 321

Input: 140
Output: 41

Input: -123
Output: -321

Input: 0
Output: 0
```

这里是策略

+   首先，将输入数字变为正数

+   使用下面的逻辑遍历数字，每次获取最后一位数字。使用最后一位数字来创建**reversedDigit**

```
for x > 0 {
	lastDigit := x % 10
	reversedDigit = reversedDigit*10 + lastDigit

	x = x / 10
}
```

这里是完整的程序。

## **程序**

```
package main

import (
	"fmt"
	"math"
)

func main() {
	reversedInteger := reverse(123)
	fmt.Println(reversedInteger)

	reversedInteger = reverse(140)
	fmt.Println(reversedInteger)

	reversedInteger = reverse(-123)
	fmt.Println(reversedInteger)

	reversedInteger = reverse(0)
	fmt.Println(reversedInteger)
}

func reverse(x int) int {
	sign := "positive"
	if x >= 0 {
		sign = "positive"
	} else {
		sign = "negative"
	}

	x = int(math.Abs(float64(x)))

	var reversedDigit int

	for x > 0 {
		lastDigit := x % 10
		reversedDigit = reversedDigit*10 + lastDigit

		x = x / 10
	}

	if sign == "negative" {
		reversedDigit = reversedDigit * -1
	}
	return reversedDigit
}
```

**输出**

```
321
41
-321
0
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
