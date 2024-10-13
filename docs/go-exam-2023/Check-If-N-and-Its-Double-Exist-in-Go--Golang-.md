<!--yml

类别：未分类

日期：2024-10-13 06:51:12

-->

# 检查N及其双倍是否存在于Go (Golang)中

> 来源：[https://golangbyexample.com/number-double-golang/](https://golangbyexample.com/number-double-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

给定一个数组。目标是找出是否存在任何数字，其双倍也存在。

**示例1**

```
Input: [8,5,4,3]
Output: true
Explanation: 4 and 8
```

**示例2**

```
Input: [1,3,7,9]
Output: false
Explanation: There exists no number for which its double exist
```

这个思路是使用一个映射。对于每个元素，我们将检查并返回true如果

+   如果它的双倍存在于映射中

+   如果数字是偶数，那么如果它的一半存在于映射中

# **程序**

下面是相同程序

```
package main

import "fmt"

func checkIfExist(arr []int) bool {
	numMap := make(map[int]bool)

	for i := 0; i < len(arr); i++ {
		if numMap[arr[i]*2] {
			return true
		}
		if arr[i]%2 == 0 && numMap[arr[i]/2] {
			return true
		}
		numMap[arr[i]] = true
	}
	return false
}

func main() {
	output := checkIfExist([]int{8, 5, 4, 3})
	fmt.Println(output)

	output = checkIfExist([]int{1, 3, 7, 9})
	fmt.Println(output)

}
```

**输出：**

```
true
false
```

**注意：** 请查看我们的Golang高级教程。该系列教程详细且我们努力涵盖所有概念及示例。此教程适合希望获得Golang专业知识和扎实理解的读者 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果您有兴趣了解所有设计模式如何在Golang中实现。如果是，那么这篇文章适合您 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

此外，您还可以查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
