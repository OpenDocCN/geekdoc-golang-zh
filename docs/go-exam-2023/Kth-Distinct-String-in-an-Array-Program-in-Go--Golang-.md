<!--yml

分类: 未分类

日期: 2024-10-13 06:49:33

-->

# 在 Go (Golang)中查找第 k 个不同字符串的程序

> 来源：[`golangbyexample.com/kth-distinct-string-golang/`](https://golangbyexample.com/kth-distinct-string-golang/)

目录

+   概述

+   程序

## **概述**

给定一个输入字符串数组，可以包含重复字符串。还提供一个输入数字 **k**。其思路是找到给定输入字符串数组中的第 k 个不同字符串

让我们通过一个例子来理解

```go
Input: ["a", "c", "b" , "c", "a", "b", "e", "d"]
k=2

Output: "d"
```

在上面的数组中，下面的字符串是重复的

+   “a”

+   “c”

+   “b”

上述字符串未重复

+   “e”

+   “d”

由于字符串 **“d”** 在顺序中第二次出现，而 k 为 2，因此输出为 **“d”**另一个例子

```go
Input: ["xxx", "xx" "x"]
k=2

Output: "xx"
```

以上原因相同

## **程序**

这里是相同的程序。

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

**注意：** 查看我们的 Golang 高级教程。此系列的教程内容详尽，我们尽力涵盖所有概念及示例。这个教程适合那些希望获得 Golang 专业知识和扎实理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是，那么这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


