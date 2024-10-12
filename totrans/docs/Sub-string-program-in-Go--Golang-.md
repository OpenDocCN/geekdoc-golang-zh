<!--yml
category: 未分类
date: 2024-10-13 06:47:04
-->

# Sub string program in Go (Golang)

> 来源：[https://golangbyexample.com/sub-string-golang/](https://golangbyexample.com/sub-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

In this tutorial, we will see the most simple way to find a substring in a given string. Note that it may not be the most efficient strategy.

The strategy will be

*   Match the substring starting with every index in the given string

Overall Time Complexity of this approach will be **O(mn)** where mis the length of the substring and **n** is the size of the input string

Our program will return the index in the original string where the given substring starts. If the substring did not exist in the given string it will return -1

Example

```
Input: "lion"
Substring: "io"
Output: 1
```

**“io”** exists in **“lion”** at position 1

Another example

**“tus”** doesn’t exist in **“tub”**

## **Program**

Here is the program for the same.

```
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

**Output**

```
1
-1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*