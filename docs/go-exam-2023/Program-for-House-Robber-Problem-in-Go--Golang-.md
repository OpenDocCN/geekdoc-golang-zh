<!--yml

类别：未分类

日期：2024-10-13 06:49:29

-->

# 用 Go (Golang) 编写的房屋抢劫问题程序

> 来源：[`golangbyexample.com/house-robber-golang/`](https://golangbyexample.com/house-robber-golang/)

目录

**概述**

+   程序 *  * ## **概述**

邻里中有几所房子。每所房子里都有一些钱。这些房子被表示为一个数组，数组中的每个条目表示该房子里的钱数。

例如，如果我们有以下数组

```go
[2, 3, 4, 2]
```

然后

+   **第一**所房子里有 **2** 元钱。

+   **第二**所房子里有 **3** 元钱。

+   **第三**所房子里有 **4** 元钱。

+   **第四**所房子里有 **2** 元钱。

抢劫者可以抢劫任意数量的房子，但他不能抢劫两个相邻的房子。例如，他可以在上述数组的以下组合中进行抢劫。

+   1 和 3

+   1 和 4

+   2 和 4

上述组合中没有相邻的房子。问题是确定哪个组合能为抢劫者带来最大的收益。

例如，在上述情况下，第一个组合（1 和 3）将给他带来最大的抢劫金额，即 2+4=6。因此，抢劫者可以在第一和第三所房子中抢劫，得到 2+4=6。

另一个例子

```go
Input: [1, 6, 8, 2, 3, 4]
Output: 13
```

抢劫者可以在第一、第三和第六所房子中抢劫，得到 1+8+4=13。

这是一个动态规划问题，因为它具有最优子结构。假设数组的名称是 **money**。

+   dp[0] = money[0]

+   dp[1] = max(money[0], money[1])

+   dp[2] = max(money[0] + money[1], money[2])

+   dp[i] = dp[i] + max(dp[i-1], dp[i-2])

其中 **dp[i]** 表示如果包括第 i 所房子，抢劫者可以抢劫的金额。最后，我们返回 **dp** 数组中的最大值。

## **程序**

这里是相同程序的代码。

```go
package main

import "fmt"

func rob(nums []int) int {
	lenNums := len(nums)

	if lenNums == 0 {
		return 0
	}

	maxMoney := make([]int, lenNums)

	maxMoney[0] = nums[0]

	if lenNums > 1 {
		maxMoney[1] = nums[1]
	}

	if lenNums > 2 {
		maxMoney[2] = nums[2] + nums[0]
	}

	for i := 3; i < lenNums; i++ {
		if maxMoney[i-2] > maxMoney[i-3] {
			maxMoney[i] = nums[i] + maxMoney[i-2]
		} else {
			maxMoney[i] = nums[i] + maxMoney[i-3]
		}

	}

	max := 0
	for i := lenNums; i < lenNums; i++ {
		if maxMoney[i] > max {
			max = maxMoney[i]
		}
	}

	return max
}

func main() {
	output := rob([]int{2, 3, 4, 2})
	fmt.Println(output)

	output = rob([]int{1, 6, 8, 2, 3, 4})
	fmt.Println(output)

}
```

**输出**

```go
6
13
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们试图用例子涵盖所有概念。这个教程是为那些希望获得专业知识和对 Golang 有深入理解的人准备的 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)。


