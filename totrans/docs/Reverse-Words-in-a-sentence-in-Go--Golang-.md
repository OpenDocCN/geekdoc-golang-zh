<!--yml
category: 未分类
date: 2024-10-13 06:47:38
-->

# Reverse Words in a sentence in Go (Golang)

> 来源：[https://golangbyexample.com/reverse-words-sentence-golang/](https://golangbyexample.com/reverse-words-sentence-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to reverse words in a given sentence

Examples

```
Input: "hello world"
Output: "word hello"
```

Another example. If the input contains a single word then that word will be returned.

```
Input: "hello"
Output: "hello"
```

Here is the strategy

*   First, we reverse the entire string. So for “hello world” it becomes

```
"dlrow olleh"
```

*   Then we reverse each word

```
"world hello"
```

*   We also need to take care of extra spaces at the end or at the start.

## **Program**

Here is the program for the same.

```
package main

import (
	"fmt"
	"regexp"
	"strings"
)

func reverseWords(s string) string {

	runeArray := []rune(s)
	length := len(runeArray)

	reverseRuneArray := reverse(runeArray)

	for i := 0; i < length; {
		for i < length && string(reverseRuneArray[i]) == " " {
			i++
		}
		if i == length {
			break
		}
		wordStart := i

		for i < length && string(reverseRuneArray[i]) != " " {
			i++
		}

		wordEnd := i - 1

		reverseRuneArray = reverseIndex(reverseRuneArray, wordStart, wordEnd)

	}

	noSpaceString := strings.TrimSpace(string(reverseRuneArray))
	space := regexp.MustCompile(`\s+`)
	return space.ReplaceAllString(noSpaceString, " ")
}

func reverse(s []rune) []rune {
	length := len(s)
	start := 0
	end := length - 1
	for start < end {
		s[start], s[end] = s[end], s[start]
		start++
		end--
	}
	return s
}

func reverseIndex(s []rune, i, j int) []rune {

	start := i
	end := j
	for start < end {
		s[start], s[end] = s[end], s[start]
		start++
		end--
	}
	return s
}

func main() {
	output := reverseWords("hello world")
	fmt.Println(output)

	output = reverseWords("hello")
	fmt.Println(output)
}
```

**Output**

```
world hello
hello
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)*