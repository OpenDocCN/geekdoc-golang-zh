<!--yml
category: 未分类
date: 2024-10-13 06:11:15
-->

# Higher-Order Functions in Go (Golang)

> 来源：[https://golangbyexample.com/gohigher-order-functions/](https://golangbyexample.com/gohigher-order-functions/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")
    *   [Example 1](#Example_1 "Example 1")
    *   [Example 2:](#Example_2 "Example 2:")*  *# **Overview**

Higher-order functions are those functions that either accept a function as a type or return function. Since a function is a first-order variable in Golang they can be passed around and also returned from some function and assigned to a variable.

# **Code:**

In below Example 1

*   **print** function takes a function of type **func(int, int) int** as an argument

*   **getAreafunc** returns a function of type **func(int, int) int**

## **Example 1**

```
package main

import "fmt"

func main() {
    areaF := getAreaFunc()
    print(3, 4, areaF)
}

func print(x, y int, area func(int, int) int) {
    fmt.Printf("Area is: %d\n", area(x, y))
}

func getAreaFunc() func(int, int) int {
    return func(x, y int) int {
        return x * y
    }
}
```

**Output:**

```
12
```

## **Example 2:**

Let’s see one more little complex example where

*   Two functions are passed as an argument
*   Two functions are returned from a function

```
package main

import "fmt"

func main() {
    add, subtract := getAddSubtract()
    print(3, 4, add, subtract)
}

func print(x, y int, add func(int, int) int, subtract func(int, int) int) {
    fmt.Printf("Sum is: %d\n", add(x, y))
    fmt.Printf("Difference Value is: %d\n", subtract(x, y))
}

func getAddSubtract() (func(int, int) int, func(int, int) int) {
    add := func(x, y int) int {
        return x + y
    }
    subtract := func(x, y int) int {
        return x - y
    }
    return add, subtract
}
```

**Output**:

```
Sum is: 7
Difference Value is: 1
```*