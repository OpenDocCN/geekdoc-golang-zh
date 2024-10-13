<!--yml

类别：未分类

日期：2024-10-13 06:43:24

-->

# 在Go语言中查找字符串内最长有效括号子串

> 来源：[https://golangbyexample.com/longest-valid-parentheses-substring-go/](https://golangbyexample.com/longest-valid-parentheses-substring-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是找到字符串内最长有效括号子串的长度

例如

```go
Input: ()(()
Output: 2

Input: ()()())
Output: 6 
```

## **程序**

```go
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

**输出**

```go
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

**注意：** 请查看我们的Golang高级教程。本系列的教程内容详尽，力求用示例覆盖所有概念。本教程适合那些希望获得Golang专业知识和扎实理解的人 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在Golang中实现所有设计模式。如果是的话，这篇文章就是为你准备的 – [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
