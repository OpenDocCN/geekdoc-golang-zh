<!--yml

分类：未分类

日期：2024-10-13 06:47:04

-->

# Go 中的子字符串程序 (Golang)

> 来源：[`golangbyexample.com/sub-string-golang/`](https://golangbyexample.com/sub-string-golang/)

目录

**   概述

+   程序*  *## **概述**

在本教程中，我们将看到在给定字符串中查找子字符串的最简单方法。请注意，这可能不是最有效的策略。

策略将是

+   从给定字符串中的每个索引开始匹配子字符串。

这种方法的整体时间复杂度为**O(mn)**，其中 m 是子字符串的长度，**n**是输入字符串的大小。

我们的程序将返回给定子字符串在原始字符串中开始的索引。如果给定字符串中不存在该子字符串，则返回-1。

示例

```go
Input: "lion"
Substring: "io"
Output: 1
```

**“io”** 在 **“lion”** 的位置 1 处存在。

另一个示例

**“tus”** 在 **“tub”** 中不存在。

## **程序**

这是相同程序的代码。

```go
package main

import "fmt"

func strStr(haystack string, needle string) int {
	runeHayStack := []rune(haystack)
	runeNeedle := []rune(needle)
	lenHayStack := len(runeHayStack)
	lenNeedle := len(runeNeedle)

	if lenNeedle == 0 && lenHayStack == 0 {
		return 0
	}

	if lenNeedle > lenHayStack {
		return -1
	}

	for i := 0; i <= lenHayStack-lenNeedle; i++ {
		k := i
		j := 0

		for j < lenNeedle {
			if runeHayStack[k] == runeNeedle[j] {
				k = k + 1
				j = j + 1
			} else {
				break
			}
		}

		if j == lenNeedle {
			return i
		}
	}

	return -1

}

func main() {
	output := strStr("lion", "io")
	fmt.Println(output)

	output = strStr("tub", "tus")
	fmt.Println(output)
}
```

**输出**

```go
1
-1
```

**注意：** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尽量用实例覆盖所有概念。本教程适合那些希望掌握 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是这样，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
