<!--yml

分类：未分类

日期：2024-10-13 06:51:17

-->

# 用 Go 语言编写的所得税计算程序

> 来源：[`golangbyexample.com/income-tax-bracket-program-golang/`](https://golangbyexample.com/income-tax-bracket-program-golang/)

目录

+   概述

+   程序

# **概述**

给定一个二维数组，表示所得税的税率区间。输入数组为**税率区间**，其中

```go
brackets[i] = [upperi, percenti]
```

这意味着第 i 个区间的上限为**upperi**，税率为**percent。** 税率区间数组按上限排序。以下是计算税率的方法

+   upper0 以下的金额税率为 percent0

+   upper1-upper0 的税率为 percent1

+   .. 以此类推

你还需要输入**收入**。你需要计算该收入的所得税。已知最后一个区间的上限大于收入。

**示例 1**

```go
Input: brackets = [[4,10],[9,20],[12,30]], income = 10
Output: 1.7
```

**示例 2**

```go
Input: brackets = [[3,10]], income = 1
Output: 0.3
```

# **程序**

下面是该程序的内容

```go
package main

import "fmt"

func calculateTax(brackets [][]int, income int) float64 {
	if income == 0 {
		return 0
	}

	var totalTax float64

	numBrackets := len(brackets)
	upper := 0
	for i := 0; i < numBrackets; i++ {

		if i == 0 {
			upper = brackets[i][0]
		} else {
			upper = brackets[i][0] - brackets[i-1][0]
		}

		taxPer := brackets[i][1]
		if income <= upper {
			totalTax += float64(income) * float64(taxPer) / 100
			break
		} else {
			totalTax += float64(upper) * float64(taxPer) / 100
			income = income - upper
		}
	}

	return totalTax
}

func main() {
	output := calculateTax([][]int{{4, 10}, {9, 20}, {12, 30}}, 10)
	fmt.Println(output)

	output = calculateTax([][]int{{3, 10}}, 10)
	fmt.Println(output)

}
```

**输出：**

```go
1.7
0.3
```

**注意：**查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试用示例覆盖所有概念。该教程适合希望获得 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

另外，您可以在这里查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)*
