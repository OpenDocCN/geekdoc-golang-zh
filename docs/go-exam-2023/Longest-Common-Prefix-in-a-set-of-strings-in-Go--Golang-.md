<!--yml

类别：未分类

日期：2024-10-13 06:50:09

-->

# Go（Golang）中的字符串集合的最长公共前缀

> 来源：[https://golangbyexample.com/longest-common-prefix-golang/](https://golangbyexample.com/longest-common-prefix-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

给定一个字符串数组，目标是找到该数组中最长的公共前缀。如果没有公共前缀，应该输出空字符串。

示例 1

```
Input: ["fan", "fat", "fame"]
Output: "fa"
```

示例 2

```
Input: ["bat", "van", "cat"]
Output: ""
```

## **程序**

以下是相同的程序

```
package main

import "fmt"

func longestCommonPrefix(strs []string) string {
	lenStrs := len(strs)

	if lenStrs == 0 {
		return ""
	}

	firstString := strs[0]

	lenFirstString := len(firstString)

	commonPrefix := ""
	for i := 0; i < lenFirstString; i++ {
		firstStringChar := string(firstString[i])
		match := true
		for j := 1; j < lenStrs; j++ {
			if (len(strs[j]) - 1) < i {
				match = false
				break
			}

			if string(strs[j][i]) != firstStringChar {
				match = false
				break
			}

		}

		if match {
			commonPrefix += firstStringChar
		} else {
			break
		}
	}

	return commonPrefix
}

func main() {
	output := longestCommonPrefix([]string{"fan", "fat", "fame"})
	fmt.Println(output)

	output = longestCommonPrefix([]string{"bat", "van", "cat"})
	fmt.Println(output)
}
```

**输出：**

```
"fa"
""
```

**注意：** 请查看我们的Golang高级教程。本系列的教程内容详尽，力求涵盖所有概念及示例。此教程适合希望获得专业知识和扎实理解的Golang学习者 - [Golang综合教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现，那么这篇文章就是为你准备的 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

此外，可以查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*
