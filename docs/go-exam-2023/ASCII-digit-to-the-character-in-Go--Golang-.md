<!--yml

分类：未分类

日期：2024-10-13 06:33:46

-->

# ASCII 数字转换为 Go 语言中的字符

> 来源：[`golangbyexample.com/ascii-digit-character/`](https://golangbyexample.com/ascii-digit-character/)

目录

**   概述

+   程序*  *# **概述**

以下是一个简单的程序，用于将 ASCII 数字转换为其对应的字符。我们可以简单地将数字类型转换为字符串，这样就能将其转换为对应的 ASCII 字符。

# **程序**

```go
package main

import "fmt"

func main() {
	sampleASCIIDigits := []int{97, 98, 99}
	for _, digit := range sampleASCIIDigits {
		fmt.Printf("Char %s\n", string(digit))
	}
}
```

**输出**

```go
Char a
Char b
Char c
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
