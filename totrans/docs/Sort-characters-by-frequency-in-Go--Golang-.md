<!--yml
category: 未分类
date: 2024-10-13 06:51:22
-->

# Sort characters by frequency in Go (Golang)

> 来源：[https://golangbyexample.com/sort-characters-frequency-go/](https://golangbyexample.com/sort-characters-frequency-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

An input string is given. The objective is to sort the string based on frequency. We have to sort based on decreasing order of the frequency of characters. Let’s understand it with an example

**Example 1**

```
Input: "bcabcb"
Output: "bbbcca"
```

**Example 2**

```
Input: "mniff"
Output: "ffmni"
```

# **Program**

Below is the program for the same

```
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

**Output:**

```
bbbcca
ffmni
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*