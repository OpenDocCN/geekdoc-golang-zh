<!--yml

分类：未分类

日期：2024-10-13 06:48:14

-->

# 用Go (Golang) 编写的最佳买卖股票程序

> 来源：[https://golangbyexample.com/buy-sell-stock-golang/](https://golangbyexample.com/buy-sell-stock-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

给定一个数组**prices**，其中**prices[i]**表示第i天的股票价格。你只能买入和卖出一次。找出通过一次买入和一次卖出可以获得的最大利润。

示例

```go
Input: [4,2,3,8,1]
Output: 6
Buy on the second day at 2 and sell on the 4th day at 8\. Profit = 8-2 = 6
```

原始顺序应予以保留

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func maxProfit(prices []int) int {
	lenPrices := len(prices)

	buy := -1
	sell := -1

	maxProphit := 0

	for i := 0; i < lenPrices; {
		for i+1 < lenPrices && prices[i] > prices[i+1] {
			i++
		}

		if i == lenPrices-1 {
			return maxProphit
		}

		buy = i

		i++

		for i+1 < lenPrices && prices[i] < prices[i+1] {
			i++
		}

		sell = i

		if (prices[sell] - prices[buy]) > maxProphit {
			maxProphit = prices[sell] - prices[buy]
		}
		i++
	}

	return maxProphit
}

func main() {
	output := maxProfit([]int{4, 2, 3, 8, 1})
	fmt.Println(output)
}
```

**输出**

```go
6
```

**注意：** 查看我们的Golang高级教程。本系列教程内容详尽，我们努力覆盖所有概念并提供示例。这个教程是为那些希望获得专业知识和深入理解Golang的人准备的——[Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在Golang中实现所有设计模式。 如果是的话，这篇文章就是为你准备的——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
