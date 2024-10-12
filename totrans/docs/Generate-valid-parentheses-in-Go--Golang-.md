<!--yml
category: 未分类
date: 2024-10-13 06:42:44
-->

# Generate valid parentheses in Go (Golang)

> 来源：[https://golangbyexample.com/generate-valid-parentheses-golang/](https://golangbyexample.com/generate-valid-parentheses-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")*  *## **Overview**

Given an int n which means the number of pairs of parentheses, generate all valid well-formed parenthesis.

For eg

```
Input:1
Output: [()]

Input:2
Output: [()(), (())]

Input:2
Output: [()()() ()(()) (())() (()()) ((()))]
```

**Program**Idea is to use two integers

*   open – It represents the number of left parentheses used so far

*   close – It represents the number of right parentheses used so far

We add the left parentheses only when

```
open < n //where n is the number of pairs of parentheses
```

We add the right parentheses only when

```
close < open //Number of right parentheses is less than the number of left parentheses
```

Below is the program for the same

```
package main

import "fmt"

func main() {
	output := generateParenthesis(1)
	fmt.Println(output)

	output = generateParenthesis(2)
	fmt.Println(output)

	output = generateParenthesis(3)
	fmt.Println(output)
}

func generateParenthesis(n int) []string {
	input := ""
	for i := 0; i < n; i++ {
		input = input + "  "
	}
	output := generateParenthesisUtil(0, n, 0, 0, []rune(input))
	return output
}

func generateParenthesisUtil(pos, n, open, close int, input []rune) []string {

	var output []string
	if pos == n*2 {
		output = append(output, string(input))
		return output
	}

	if close < open {
		input[pos] = ')'
		result := generateParenthesisUtil(pos+1, n, open, close+1, input)
		output = append(output, result...)

	}

	if open < n {
		input[pos] = '('
		result := generateParenthesisUtil(pos+1, n, open+1, close, input)
		output = append(output, result...)
	}

	return output
}
```

**Output**

```
[()]
[()() (())]
[()()() ()(()) (())() (()()) ((()))]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*