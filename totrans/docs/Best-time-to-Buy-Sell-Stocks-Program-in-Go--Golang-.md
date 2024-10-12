<!--yml
category: 未分类
date: 2024-10-13 06:48:14
-->

# Best time to Buy-Sell Stocks Program in Go (Golang)

> 来源：[https://golangbyexample.com/buy-sell-stock-golang/](https://golangbyexample.com/buy-sell-stock-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An array **prices** is given where **prices[i]** represent the price of a stock on the ith day.  You can only buy and sell once. Find the max profit you can earn by buying and selling the stock once.

Example

```
Input: [4,2,3,8,1]
Output: 6
Buy on the second day at 2 and sell on the 4th day at 8\. Profit = 8-2 = 6
```

The original order should be preserved as well

## **Program**

Here is the program for the same.

```
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

**Output**

```
6
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*