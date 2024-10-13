<!--yml

分类：未分类

日期：2024-10-13 06:43:34

-->

# Go 语言中的中缀到后缀转换

> 来源：[`golangbyexample.com/infix-to-postfix-conversion-go/`](https://golangbyexample.com/infix-to-postfix-conversion-go/)

目录

**   中缀到后缀转换

+   算法：

+   实现：

## **中缀到后缀转换**

在本教程中，我们将了解中缀和后缀表达式的表示法，后缀表示法相比中缀的优点，以及如何将中缀表达式转换为后缀表达式。我们将在另一个教程中涵盖后缀表达式的计算。

```go
Infix Expression: In infix expression, the operator is in between pair of operands like (a op b).
example: a+b, 2/2 .
Postfix Expression: In postfix expression, the operator is placed post to both operands like (a b op).
example: ab+, 22/ . 
```

这些是后缀表达式相比中缀表达式的一些优点：

+   后缀表达式的计算比中缀表达式的计算更简单。

+   后缀表达式中不需要括号。

+   后缀表达式的计算只需单次扫描。

+   比中缀表示法更快的计算。

## **算法：**

+   从左到右扫描中缀表达式。

+   如果当前扫描的字符是操作数，输出它。

+   否则，如果当前扫描的运算符的优先级高于栈中运算符的优先级（或栈为空或栈中包含一个‘(’），则压入栈。否则，从栈中弹出所有优先级大于或等于扫描运算符的运算符。完成后将扫描到的运算符推入栈中。（如果在弹出时遇到括号，则停止，并将扫描到的运算符压入栈中。）

+   如果扫描到的字符是‘(’，则将其压入栈中。

+   如果扫描到的字符是‘)’，则弹出栈并输出，直到遇到‘(’，并丢弃这两个括号。

+   重复步骤 2-6，直到中缀表达式扫描完成。

+   打印输出

+   弹出并输出栈中的内容，直到栈为空。

以下是上述算法的示例：

中缀表达式为 a+b，则后缀表达式为 ab+：

+   a 是操作数（输出它） //output=a

+   + 是运算符（推入栈中）

+   b 是操作数（输出它） //output=ab

+   中缀表达式已完全扫描，现在从栈中弹出并添加到输出，因此后缀变为 ab+

类似地

中缀表达式为

```go
a+b*c+d
```

然后后缀表达式为

```go
abc*+d+:
```

+   a 是操作数（输出它） //output=a

+   + 是运算符（压入栈中） //stack=+

+   b 是操作数（输出它） //ab

+   * 是运算符，优先级高于 +，（压入栈中） //stack=+*

+   c 是操作数（输出它） //output=abc

+   + 是运算符，但其优先级低于栈中的运算符。弹出栈中的所有运算符并压入 + //output=abc*+, stack=+

+   d 是操作数（输出它） //output=abc*+d

+   从栈中弹出并添加到输出 //output=abc*+d+

## **实现：**

以下是 Go 语言中中缀到后缀转换的实现：

```go
package main

import "fmt"

type Stack []string

//IsEmpty: check if stack is empty
func (st *Stack) IsEmpty() bool {
    return len(*st) == 0
}

//Push a new value onto the stack
func (st *Stack) Push(str string) {
    *st = append(*st, str) //Simply append the new value to the end of the stack
}

//Remove top element of stack. Return false if stack is empty.
func (st *Stack) Pop() bool {
    if st.IsEmpty() {
        return false
    } else {
        index := len(*st) - 1 // Get the index of top most element.
        *st = (*st)[:index]   // Remove it from the stack by slicing it off.
        return true
    }
}

//Return top element of stack. Return false if stack is empty.
func (st *Stack) Top() string {
    if st.IsEmpty() {
        return ""
    } else {
        index := len(*st) - 1   // Get the index of top most element.
        element := (*st)[index] // Index onto the slice and obtain the element.
        return element
    }
}

//Function to return precedence of operators
func prec(s string) int {
    if s == "^" {
        return 3
    } else if (s == "/") || (s == "*") {
        return 2
    } else if (s == "+") || (s == "-") {
        return 1
    } else {
        return -1
    }
}

func infixToPostfix(infix string) string {
    var sta Stack
    var postfix string
    for _, char := range infix {
        opchar := string(char)
        // if scanned character is operand, add it to output string
        if (char >= 'a' && char <= 'z') || (char >= 'A' && char <= 'Z') || (char >= '0' && char <= '9') {
            postfix = postfix + opchar
        } else if char == '(' {
            sta.Push(opchar)
        } else if char == ')' {
            for sta.Top() != "(" {
                postfix = postfix + sta.Top()
                sta.Pop()
            }
            sta.Pop()
        } else {
            for !sta.IsEmpty() && prec(opchar) <= prec(sta.Top()) {
                postfix = postfix + sta.Top()
                sta.Pop()
            }
            sta.Push(opchar)
        }
    }
    // Pop all the remaining elements from the stack
    for !sta.IsEmpty() {
        postfix = postfix + sta.Top()
        sta.Pop()
    }
    return postfix
}
func main() {
    //infix := "a+b"
    //infix := "a+b*c+d"
    //infix := "a+b*(c^d-e)^(f+g*h)-i" // abcd^e-fgh*+^*+i-
    //infix := "1+2+3*4+5/5-2"
    infix := "2+3*(2³-5)^(2+1*2)-4" //abcd^e-fgh*+^*+i-
    postfix := infixToPostfix(infix)
    fmt.Printf("%s infix has %s postfix ", infix, postfix)

}
```

**输出：**

```go
2+3*(2³-5)^(2+1*2)-4 infix has 2323⁵-212*+^*+4- postfix
```

我们可以通过取消注释**fmt.Println**行来检查每次推送和弹出操作后栈的状态。

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽量用例子覆盖所有概念。本教程适合那些希望获得 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
