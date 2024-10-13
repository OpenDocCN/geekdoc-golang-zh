<!--yml

类别：未分类。

日期：2024-10-13 06:42:49

-->

# 检查 Go (Golang) 中的有效括号

> 来源：[`golangbyexample.com/valid-parenthesis-golang/`](https://golangbyexample.com/valid-parenthesis-golang/)

目录

**   概述

+   程序*  *## **概述**

有一个输入字符串，只包含以下字符。

+   (

+   )

+   {

+   }

+   [

+   ]

**目标是检查输入字符串是否具有有效的括号。一个括号是有效的，如果**

***   开放括号应该按照相同的顺序闭合。这意味着下面的内容将是无效的，因为括号没有按照正确的顺序闭合。

([)]

+   每个开放括号都应有对应的闭合括号。

有效括号示例。

```go
()
{}
[]
()[]
([{}])
()[{}]
```

无效括号示例。

```go
([)]
(
{
[
(()
```

## **程序**

思路是使用栈。

+   如果给定的括号是左括号，我们将其推入栈中。

+   如果给定的括号是右括号，我们将其从栈中弹出，并检查弹出的左括号是否与当前的右括号对应。例如，如果我们在字符串中遇到**‘]’**，我们弹出并检查弹出的值是否仅为**‘[‘**。如果不是，则返回 false。**‘)’**和**‘}’**也同理。

+   在字符串的末尾，栈应该是空的。

以下是相同的程序。

```go
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

**输出**

```go
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

**注意：** 查看我们的 Golang 高级教程。该系列教程内容详尽，我们试图用示例覆盖所有概念。本教程适合那些希望获得 Golang 专业知识和扎实理解的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)***
