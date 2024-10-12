<!--yml
category: 未分类
date: 2024-10-13 06:42:49
-->

# Check valid parenthesis in Go (Golang)

> 来源：[https://golangbyexample.com/valid-parenthesis-golang/](https://golangbyexample.com/valid-parenthesis-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

There is an input string that only contains below characters

*   (

*   )

*   {

*   }

*   [

*   ]

**The objective is to check if the input string has valid parentheses. A parenthesis is valid if**

 ***   Open parentheses should be closed in the same order. Meaning that the below will be invalid as parentheses are not closed in right order

([)]

*   Every open parenthesis should have the corresponding close parentheses

Valid parentheses example

```
()
{}
[]
()[]
([{}])
()[{}]
```

Invalid parentheses example

```
([)]
(
{
[
(()
```

## **Program**

Idea is to use a stack

*   If the given parentheses is a left parenthesis we push it to the stack

*   If the given parentheses is right parentheses we pop from the stack and we check that the popped left parentheses is corresponding to the current right parentheses. For eg if the if we encounter **‘]’** in the string we pop and check if the popped value is **‘[‘** only. If it is not we return false. Same for **‘)’** and **‘}’**

*   At the end of the string, the stack should be empty

Below is the program for the same.

```
package main

import (
	"container/list"
	"fmt"
)

func main() {
	valid := isValid("()")
	fmt.Println(valid)

	valid = isValid("[]")
	fmt.Println(valid)

	valid = isValid("{}")
	fmt.Println(valid)

	valid = isValid("()[]")
	fmt.Println(valid)

	valid = isValid("([{}])")
	fmt.Println(valid)

	valid = isValid("()[{}]")
	fmt.Println(valid)

	valid = isValid("([)]")
	fmt.Println(valid)

	valid = isValid("(")
	fmt.Println(valid)

	valid = isValid("(()")
	fmt.Println(valid)

}

type customStack struct {
	stack *list.List
}

func (c *customStack) Push(value string) {
	c.stack.PushFront(value)
}

func (c *customStack) Pop() error {
	if c.stack.Len() > 0 {
		ele := c.stack.Front()
		c.stack.Remove(ele)
	}
	return fmt.Errorf("Pop Error: Stack is empty")
}

func (c *customStack) Front() (string, error) {
	if c.stack.Len() > 0 {
		if val, ok := c.stack.Front().Value.(string); ok {
			return val, nil
		}
		return "", fmt.Errorf("Peep Error: Stack Datatype is incorrect")
	}
	return "", fmt.Errorf("Peep Error: Stack is empty")
}

func (c *customStack) Size() int {
	return c.stack.Len()
}

func (c *customStack) Empty() bool {
	return c.stack.Len() == 0
}

func isValid(s string) bool {
	customStack := &customStack{
		stack: list.New(),
	}

	for _, val := range s {

		if val == '(' || val == '[' || val == '{' {
			customStack.Push(string(val))
		} else if val == ')' {
			poppedValue, _ := customStack.Front()
			if poppedValue != "(" {
				return false
			}
			customStack.Pop()
		} else if val == ']' {
			poppedValue, _ := customStack.Front()
			if poppedValue != "[" {
				return false
			}
			customStack.Pop()

		} else if val == '}' {
			poppedValue, _ := customStack.Front()
			if poppedValue != "{" {
				return false
			}
			customStack.Pop()

		}

	}

	return customStack.Size() == 0
}
```

**Output**

```
true
true
true
true
true
true
false
false
false
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)***