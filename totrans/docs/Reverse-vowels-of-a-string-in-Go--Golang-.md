<!--yml
category: 未分类
date: 2024-10-13 06:50:52
-->

# Reverse vowels of a string in Go (Golang)

> 来源：[https://golangbyexample.com/reverse-vowels-string-golang/](https://golangbyexample.com/reverse-vowels-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

A string is given. The objective is to reverse all vowels in that string

**Example 1**

```
Input: "simple"
Output: "sempli"
```

**Example 2**

```
Input: "complex"
Output: "cemplox"
```

# **Program**

Below is the program for the same

```
package main

import "fmt"

func reverseVowels(s string) string {
	runeS := []rune(s)
	lenS := len(runeS)

	for i, j := 0, lenS-1; i < j; {
		for i < j {
			if !vowel(runeS[i]) {
				i++
			} else {
				break
			}
		}
		if i == j {
			break
		}
		for i < j {
			if !vowel(runeS[j]) {
				j--
			} else {
				break
			}
		}

		if i == j {
			break
		}

		runeS[i], runeS[j] = runeS[j], runeS[i]
		i++
		j--

	}

	return string(runeS)

}

func vowel(s rune) bool {
	if s == 'a' || s == 'e' || s == 'i' || s == 'o' || s == 'u' {
		return true
	}

	if s == 'A' || s == 'E' || s == 'I' || s == 'O' || s == 'U' {
		return true
	}

	return false
}

func main() {
	output := reverseVowels("simple")
	fmt.Println(output)

	output = reverseVowels("complex")
	fmt.Println(output)

}
```

**Output:**

```
sempli
cemplox
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)*