<!--yml
category: 未分类
date: 2024-10-13 06:49:21
-->

# Maximum words in a group of sentences in Go (Golang)

> 来源：[https://golangbyexample.com/maximum-words-group-sentences-go/](https://golangbyexample.com/maximum-words-group-sentences-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

There is a group of sentences given. Find the maximum number of words that appear in a sentence within that group of sentences

Example

```
Input: ["Hello World", "This is hat]
Output: 3

Input: ["Hello World", "This is hat", "The cat is brown"]
Output: 4
```

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func mostWordsFound(sentences []string) int {
	lenSentences := len(sentences)

	max := 0
	for i := 0; i < lenSentences; i++ {
		countWord := countW(sentences[i])
		if countWord > max {
			max = countWord
		}
	}

	return max
}

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
	output := mostWordsFound([]string{"Hello World", "This is hat"})
	fmt.Println(output)

	output = mostWordsFound([]string{"Hello World", "This is hat", "The cat is brown"})
	fmt.Println(output)
}
```

**Output**

```
3
4
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*