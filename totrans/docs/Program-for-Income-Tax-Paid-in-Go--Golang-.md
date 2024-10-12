<!--yml
category: 未分类
date: 2024-10-13 06:51:17
-->

# Program for Income Tax Paid in Go (Golang)

> 来源：[https://golangbyexample.com/income-tax-bracket-program-golang/](https://golangbyexample.com/income-tax-bracket-program-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

A 2d array is given which represents the bracket of income tax. The input array is **brackets** where

```
brackets[i] = [upperi, percenti]
```

which means that ith bracket has an upper bound of **upperi** and is taxed at a rate of **percent.** The brackets array is sorted by upper bound. Below is the way tax is calculated

*   Money till upper0 is taxed at percent0

*   upper1-upper0 is taxed at percent1

*   .. and so on

You are also given the **income** as an input. You have to calculate income tax on that. It is given that the upper bound of the last bracket is greater than the income.

**Example 1**

```
Input: brackets = [[4,10],[9,20],[12,30]], income = 10
Output: 1.7
```

**Example 2**

```
Input: brackets = [[3,10]], income = 1
Output: 0.3
```

# **Program**

Below is the program for the same

```
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

**Output:**

```
1.7
0.3
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)*