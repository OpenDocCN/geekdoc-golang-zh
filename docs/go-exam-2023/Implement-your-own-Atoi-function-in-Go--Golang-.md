<!--yml

类别：未分类

日期：2024-10-13 06:42:17

-->

# 在 Go (Golang) 中实现你自己的 Atoi 函数

> 来源：[`golangbyexample.com/implement-your-own-atoi-function-golang/`](https://golangbyexample.com/implement-your-own-atoi-function-golang/)

目录

+   概述

+   程序

## **概述**

**Atoi** 函数将给定字符串转换为其数字表示。例如

```go
Input: "121"
Output: 121

Input: "-121"
Output: 121

Input: "0"
Output: 0
```

## **程序**

以下是相应的程序。

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	output := myAtoi("121")
	fmt.Println(output)

	output = myAtoi("-121")
	fmt.Println(output)

	output = myAtoi("0")
	fmt.Println(output)
}

func myAtoi(s string) int {

	var output int

	sign := "positive"
	if string(s[0]) == "-" {
		sign = "negtive"
		s = s[1:]
	} else if string(s[0]) == "+" {
		s = s[1:]
	}

	stringLen := len(s)

	for i := 0; i < stringLen; i++ {
		tempNum, _ := strconv.Atoi(string(s[i]))
		output = output*10 + tempNum
	}

	if sign == "negtive" {
		output = output * -1
	}

	return output
}
```

**输出**

```go
121
-121
0
```


