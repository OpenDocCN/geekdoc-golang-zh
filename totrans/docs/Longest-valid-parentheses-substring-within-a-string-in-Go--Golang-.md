<!--yml
category: 未分类
date: 2024-10-13 06:43:24
-->

# Longest valid parentheses substring within a string in Go (Golang)

> 来源：[https://golangbyexample.com/longest-valid-parentheses-substring-go/](https://golangbyexample.com/longest-valid-parentheses-substring-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the length of the longest valid parentheses substring within a string

Eg

```
Input: ()(()
Output: 2

Input: ()()())
Output: 6 
```

## **Program**

```
package main

import (
	"fmt"
	"sync"
)

func main() {
	valid := longestValidParentheses("()(()")
	fmt.Println(valid)

	valid = longestValidParentheses("()()())")
	fmt.Println(valid)

	valid = longestValidParentheses("(()")
	fmt.Println(valid)

	valid = longestValidParentheses("(()(()")
	fmt.Println(valid)

	valid = longestValidParentheses("()()")
	fmt.Println(valid)

	valid = longestValidParentheses("(()()")
	fmt.Println(valid)

	valid = longestValidParentheses("((()))(()")
	fmt.Println(valid)

	valid = longestValidParentheses(")(((((()())()()))()(()))(")
	fmt.Println(valid)

	valid = longestValidParentheses("")
	fmt.Println(valid)

}

type customStack struct {
	stack []int
	lock  sync.RWMutex
}

func (c *customStack) Push(name int) {
	c.lock.Lock()
	defer c.lock.Unlock()
	c.stack = append(c.stack, name)
}

func (c *customStack) Pop() (int, error) {
	len := len(c.stack)
	if len > 0 {
		c.lock.Lock()
		defer c.lock.Unlock()
		val := c.stack[len-1]
		c.stack = c.stack[:len-1]
		return val, nil
	}
	return 0, fmt.Errorf("Pop Error: Stack is empty")
}

func (c *customStack) Front() (int, error) {
	len := len(c.stack)
	if len > 0 {
		c.lock.Lock()
		defer c.lock.Unlock()
		return c.stack[len-1], nil
	}
	return 0, fmt.Errorf("Peep Error: Stack is empty")
}

func (c *customStack) Size() int {
	return len(c.stack)
}

func (c *customStack) Empty() bool {
	return len(c.stack) == 0
}

func (c *customStack) Flush() {
	c.stack = []int{}
}

func longestValidParentheses(s string) int {

	customStack := &customStack{
		stack: []int{},
	}

	longestValidParenthesisLength := 0
	currentValidParenthesisLength := 0
	customStack.Push(-1)
	for i, val := range s {

		if val == '(' {
			customStack.Push(i)
		} else if val == ')' {
			customStack.Pop()
			if customStack.Size() == 0 {
				customStack.Push(i)
			} else {
				index, _ := customStack.Front()
				currentValidParenthesisLength = i - index
				if currentValidParenthesisLength > longestValidParenthesisLength {
					longestValidParenthesisLength = currentValidParenthesisLength
				}
			}

		}

	}

	if currentValidParenthesisLength > longestValidParenthesisLength {
		longestValidParenthesisLength = currentValidParenthesisLength
	}

	return longestValidParenthesisLength
}
```

**Output**

```
2
6
2
2
4
4
6
22
0
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*