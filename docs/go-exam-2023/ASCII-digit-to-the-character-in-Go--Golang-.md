<!--yml

分类：未分类

日期：2024-10-13 06:33:46

-->

# ASCII数字转换为Go语言中的字符

> 来源：[https://golangbyexample.com/ascii-digit-character/](https://golangbyexample.com/ascii-digit-character/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

以下是一个简单的程序，用于将ASCII数字转换为其对应的字符。我们可以简单地将数字类型转换为字符串，这样就能将其转换为对应的ASCII字符。

# **程序**

```
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

```
Char a
Char b
Char c
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
