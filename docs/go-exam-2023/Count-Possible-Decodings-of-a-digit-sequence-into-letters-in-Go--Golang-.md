<!--yml

类别：未分类

日期：2024-10-13 06:47:48

-->

# 计算数字序列可能解码为字母的方式（Golang）

> 来源：[https://golangbyexample.com/count-possible-decodings-digit-golang/](https://golangbyexample.com/count-possible-decodings-digit-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

假设我们有以下数字到字母的映射

```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

目标是计算给定数字序列的可能解码数量

示例

```
Input: '15'
Output: 2

15 can be decoded as "AE" or "O"
```

## **程序**

这里是相应的程序。

```
package main

import (
	"fmt"
	"strconv"
)

func numDecodings(s string) int {

	runeArray := []rune(s)
	length := len(runeArray)
	if length == 0 {
		return 0
	}

	if string(runeArray[0]) == "0" {
		return 0
	}

	numwaysArray := make([]int, length)

	numwaysArray[0] = 1

	if length == 1 {
		return numwaysArray[0]
	}

	secondDigit := string(runeArray[0:2])
	num, _ := strconv.Atoi(secondDigit)
	if num > 10 && num <= 19 {
		numwaysArray[1] = 2
	} else if num > 20 && num <= 26 {
		numwaysArray[1] = 2
	} else if num == 10 || num == 20 {
		numwaysArray[1] = 1
	} else if num%10 == 0 {
		numwaysArray[1] = 0
	} else {
		numwaysArray[1] = 1
	}

	for i := 2; i < length; i++ {
		firstDigit := string(runeArray[i])
		if firstDigit != "0" {
			numwaysArray[i] = numwaysArray[i] + numwaysArray[i-1]
		}

		secondDigit := string(runeArray[i-1 : i+1])
		fmt.Println(i)
		fmt.Println(secondDigit)

		num, _ := strconv.Atoi(secondDigit)

		if num >= 10 && num <= 26 {
			numwaysArray[i] = numwaysArray[i] + numwaysArray[i-2]
		}
	}

	return numwaysArray[length-1]

}

func main() {
	output := numDecodings("15")
	fmt.Println(output)
}
```

**输出**

```
2
```

**注意：** 请查看我们的Golang高级教程。本系列教程详尽，并且我们努力涵盖所有概念及实例。本教程适合那些希望获得专业知识和扎实理解Golang的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在Golang中实现所有设计模式，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
