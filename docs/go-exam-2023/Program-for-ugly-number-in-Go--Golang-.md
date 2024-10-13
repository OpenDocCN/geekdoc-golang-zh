<!--yml

类别：未分类

日期：2024-10-13 06:51:39

-->

# 在 Go (Golang) 中的丑数程序

> 来源：[https://golangbyexample.com/ugly-number-golang/](https://golangbyexample.com/ugly-number-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

丑数是其质因数仅限于 2、3 和 5 的数字。

给定一个数字 n，如果它是一个丑数则返回 true，否则返回 false。

**示例 1**

```
Input: 12
Output: true
```

**示例 2**

```
Input: 7 
Output: false
```

思路是

+   当数字是 2 的因数时，不断将数字除以 2。

+   当数字是 3 的因数时，不断将数字除以 3。

+   当数字是 5 的因数时，不断将数字除以 5。

最后，如果数字等于 1，则返回 true，否则返回 false。

# **程序**

以下是相同的程序

```
package main

import "fmt"

func isUgly(n int) bool {
	if n == 0 {
		return false
	}
	if n == 1 {
		return true
	}

	for {
		if n%2 != 0 {
			break
		}
		n = n / 2
	}

	for {
		if n%3 != 0 {
			break
		}
		n = n / 3
	}

	for {
		if n%5 != 0 {
			break
		}
		n = n / 5
	}

	return n == 1
}

func main() {
	output := isUgly(12)
	fmt.Println(output)

	output = isUgly(7)
	fmt.Println(output)
}
```

**输出**

```
true
false
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽量涵盖所有概念并配有示例。此教程适合那些希望获得专业知识和对 Golang 有深入理解的人—— [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解所有设计模式如何在 Golang 中实现感兴趣。如果是的话，这篇文章适合你—— [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

此外，请查看我们的系统设计教程系列—— [系统设计教程系列](https://techbyexample.com/system-design-questions/)*
