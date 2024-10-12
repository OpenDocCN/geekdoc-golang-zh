<!--yml
category: 未分类
date: 2024-10-13 06:43:40
-->

# Evaluation of Postfix Expression in Go (Golang)

> 来源：[https://golangbyexample.com/evaluation-of-postfix-expression-golang/](https://golangbyexample.com/evaluation-of-postfix-expression-golang/)

Table of Contents

 **   [Evaluation of Postfix Expression](#Evaluation_of_Postfix_Expression "Evaluation of Postfix Expression")
*   [Implementation](#Implementation "Implementation")*  *## **Evaluation of Postfix Expression**

In this tutorial, we will evaluate a postfix expression.

**Algorithm:**

*   Scan postfix expression from left to right.  

*   If the current scanned character is an operand, push it in the stack.

*   If the current scanned character is an operator, then pop two operands from the stack. Evaluate the operator on the two popped operands and push the result in the stack.

*   When postfix expression is scanned completely, there should be only one value in the stack which is the result of postfix expression.

## **Implementation**

Below is the implementation of Infix to Postfix Conversion’ in golang. 

```
package main

import (
    "fmt"
    "math"
    "strconv"
)

type Stack []int

//IsEmpty: check if stack is empty
func (st *Stack) IsEmpty() bool {
    return len(*st) == 0
}

//Push a new value onto the stack
func (st *Stack) Push(data int) {
    *st = append(*st, data) //Simply append the new value to the end of the stack
    //fmt.Println("st after push", st)
}

//Remove top element of stack. Return false if stack is empty.
func (st *Stack) Pop() bool {
    if st.IsEmpty() {
        return false
    } else {
        index := len(*st) - 1 // Get the index of top most element.
        *st = (*st)[:index]   // Remove it from the stack by slicing it off.
        //fmt.Println("st after pops", *st)
        return true
    }
}

//Return top element of stack. Return false if stack is empty.
func (st *Stack) Top() int {
    if st.IsEmpty() {
        return 0
    } else {
        index := len(*st) - 1   // Get the index of top most element.
        element := (*st)[index] // Index onto the slice and obtain the element.
        return element
    }
}

//function to evaluate postfix expression
func evaluationPostfix(postfix string) int {
    var intStack Stack
    for _, char := range postfix {
        opchar := string(char)
        //fmt.Println(opchar)
        if opchar >= "0" && opchar <= "9" {
            i1, _ := strconv.Atoi(opchar)
            //fmt.Println("Integer value is: ", i1)
            intStack.Push(i1)
            //fmt.Println(intStack)
        } else {
            opr1 := intStack.Top()
            intStack.Pop()
            opr2 := intStack.Top()
            intStack.Pop()
            switch char {
            case '^':
                x := math.Pow(float64(opr2), float64(opr1))
                intStack.Push(int(x))
            case '+':
                intStack.Push(opr2 + opr1)

            case '-':
                intStack.Push(opr2 - opr1)

            case '*':
                intStack.Push(opr2 * opr1)

            case '/':
                intStack.Push(opr2 / opr1)

            }

        }
    }
    return intStack.Top()
}

func main() {
    postfix := "2323^5-212*+^*+4-"
    evaluationReslt := evaluationPostfix(postfix)
    fmt.Printf("evaluation of %s is %d", postfix, evaluationReslt)
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