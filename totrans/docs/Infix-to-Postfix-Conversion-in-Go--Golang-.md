<!--yml
category: 未分类
date: 2024-10-13 06:43:34
-->

# Infix to Postfix Conversion in Go (Golang)

> 来源：[https://golangbyexample.com/infix-to-postfix-conversion-go/](https://golangbyexample.com/infix-to-postfix-conversion-go/)

Table of Contents

 **   [Infix to Postfix Conversion ](#Infix_to_Postfix_Conversion "Infix to Postfix Conversion ")
*   [Algorithm:](#Algorithm "Algorithm:")
*   [Implementation:](#Implementation "Implementation:")*  *## **Infix to Postfix Conversion **

In this tutorial, we will see what are the Infix and Postfix expression notations, the advantages of Postfix notation over Infix, and how we can convert an Infix expression to a Postfix expression. We will cover the evaluation of a Postfix expression in another tutorial.  

```
Infix Expression: In infix expression, the operator is in between pair of operands like (a op b).
example: a+b, 2/2 .
Postfix Expression: In postfix expression, the operator is placed post to both operands like (a b op).
example: ab+, 22/ . 
```

These are some advantages of postfix expression over infix:

*   Postfix expression evaluation is easier than Infix expression evaluation.

*   Parenthesis is not required in postfix expression.

*   Postfix expression evaluation requires single scanning.

*   Faster evaluation than Infix notation.

## **Algorithm:**

*   Scan infix expression from left to right. 

*   If the current scanned character is an operand, output it. 

*   Else, If the precedence of the current scanned operator is greater than the precedence of the operator in the stack (or the stack is empty or the stack contains a ‘(‘ ), push it.  Else, Pop all the operators from the stack which are greater than or equal to in precedence than that of the scanned operator. After doing that Push the scanned operator to the stack. (If you encounter parenthesis while popping then stop there and push the scanned operator in the stack.) 

*   If the scanned character is an ‘(‘, push it to the stack. 

*   If the scanned character is an ‘)’, pop the stack and output it until a ‘(‘ is encountered, and discard both the parenthesis. 

*   Repeat steps 2-6 until infix expression is scanned. 

*   Print the output 

*   Pop and output from the stack until it is not empty.

Below are the examples of the above algorithm:

Infix expression is a+b then postfix expression is ab+ :

*   a is operand (output it) //output=a

*   + is operator (push is in the stack)

*   b is operand (output it) //output=ab

*   infix expression is scanned completely, now pop from the stack and add in output, hence postfix becomes ab+

Similarly

Infix expression is

```
a+b*c+d
```

then postfix expression is

```
abc*+d+:
```

*   a is operand (output it) //output=a
*   + is operator (push it in stack) //stack=+
*   b is operand (output it) //ab
*   * is operator and having high precedence than +, (push it in stack) //stack=+*
*   c is operand (output it) //output=abc
*   + is operator but it lower than operators in stack. pop all from stack and push + //output=abc*+, stack=+
*   d is operand (output it) //output=abc*+d
*   pop from stack and add in output //output=abc*+d+

## **Implementation:**

Below is the implementation of Infix to Postfix Conversion in golang:

```
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
    infix := "2+3*(2^3-5)^(2+1*2)-4" //abcd^e-fgh*+^*+i-
    postfix := infixToPostfix(infix)
    fmt.Printf("%s infix has %s postfix ", infix, postfix)

}
```

**Output:**

```
2+3*(2^3-5)^(2+1*2)-4 infix has 2323^5-212*+^*+4- postfix
```

We can check the status of the stack after every push and pop operation by uncommenting **fmt.Println** line in Push and Pop function declaration.

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*