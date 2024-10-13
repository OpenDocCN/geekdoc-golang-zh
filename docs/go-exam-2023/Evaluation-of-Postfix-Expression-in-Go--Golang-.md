<!--yml

分类：未分类

日期：2024-10-13 06:43:40

-->

# 在Go (Golang)中进行后缀表达式求值

> 来源：[https://golangbyexample.com/evaluation-of-postfix-expression-golang/](https://golangbyexample.com/evaluation-of-postfix-expression-golang/)

目录

**   [后缀表达式求值](#Evaluation_of_Postfix_Expression "Evaluation of Postfix Expression")

+   [实现](#Implementation "Implementation")*  *## **后缀表达式求值**

在本教程中，我们将评估一个后缀表达式。

**算法：**

+   从左到右扫描后缀表达式。

+   如果当前扫描的字符是操作数，则将其推入堆栈。

+   如果当前扫描的字符是运算符，则从堆栈中弹出两个操作数。在这两个弹出的操作数上对运算符进行求值，并将结果推入堆栈。

+   当后缀表达式完全扫描后，堆栈中应该只有一个值，这就是后缀表达式的结果。

## **实现**

以下是Golang中“中缀转后缀”的实现。

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

**输出：**

```
2+3*(2^3-5)^(2+1*2)-4 infix has 2323^5-212*+^*+4- postfix
```

我们可以通过取消注释**fmt.Println**行来检查每次推送和弹出操作后的堆栈状态。

**注意：** 查看我们的Golang高级教程。本系列教程内容详尽，我们尽力覆盖所有概念并附有示例。本教程适合希望获得专业知识和扎实理解Golang的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式在Golang中的实现。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
