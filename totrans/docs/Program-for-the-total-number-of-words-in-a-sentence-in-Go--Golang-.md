<!--yml
category: 未分类
date: 2024-10-13 06:49:17
-->

# Program for the total number of words in a sentence in Go (Golang)

> 来源：[https://golangbyexample.com/number-words-sentence-golang/](https://golangbyexample.com/number-words-sentence-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Given a sentence find number of words in it. Each word in the sentence only has English letters

Example

```
Input: "Hello World"
Output: 2

Input: "This is hat"
Output: 3
```

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func countW(s string) int {

	lenS := len(s)
	numWords := 0

	for i := 0; i < lenS; {
		for i < lenS && string(s[i]) == " " {
			i++
		}

		if i < lenS {
			numWords++
		}

		for i < lenS && string(s[i]) != " " {
			i++
		}
	}

	return numWords
}

func main() {
	output := countW("Hello World")
	fmt.Println(output)

	output = countW("This is hat")
	fmt.Println(output)
}
```

**Output**

```
2
3
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*