<!--yml

分类：未分类

日期：2024-10-13 06:51:22

-->

# 在Go (Golang)中按频率对字符进行排序

> 来源：[https://golangbyexample.com/sort-characters-frequency-go/](https://golangbyexample.com/sort-characters-frequency-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

给定一个输入字符串。目标是根据频率对字符串进行排序。我们必须按照字符频率的降序进行排序。让我们通过一个示例来理解

**示例 1**

```go
Input: "bcabcb"
Output: "bbbcca"
```

**示例 2**

```go
Input: "mniff"
Output: "ffmni"
```

# **程序**

下面是相同的程序

```go
package main

import (
	"fmt"
	"sort"
)

func frequencySort(s string) string {
	stringMap := make(map[byte]int)
	lenS := len(s)
	for i := 0; i < lenS; i++ {
		stringMap[s[i]]++
	}

	itemArray := make([]item, 0)

	for key, value := range stringMap {
		i := item{
			char:      key,
			frequency: value,
		}
		itemArray = append(itemArray, i)
	}

	sort.Slice(itemArray, func(i, j int) bool {
		return itemArray[i].frequency > itemArray[j].frequency
	})

	output := ""

	for i := 0; i < len(itemArray); i++ {
		for j := 0; j < itemArray[i].frequency; j++ {
			output = output + string(itemArray[i].char)
		}
	}

	return output

}

type item struct {
	char      byte
	frequency int
}

func main() {
	output := frequencySort("bcabcb")
	fmt.Println(output)

	output = frequencySort("mniff")
	fmt.Println(output)

}
```

**输出：**

```go
bbbcca
ffmni
```

**注意：** 请查看我们的Golang高级教程。本系列教程详细且覆盖了所有概念及示例。本教程适合那些希望获得专业知识并深入理解Golang的学习者 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解所有设计模式在Golang中的实现感兴趣，这篇文章适合你 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

同时，请查看我们的系统设计教程系列 - [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
