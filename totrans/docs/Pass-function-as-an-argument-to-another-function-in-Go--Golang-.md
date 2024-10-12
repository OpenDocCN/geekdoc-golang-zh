<!--yml
category: 未分类
date: 2024-10-13 06:10:57
-->

# Pass function as an argument to another function in Go (Golang)

> 来源：[https://golangbyexample.com/func-as-func-argument-go/](https://golangbyexample.com/func-as-func-argument-go/)

Golang function are first-order variables meaning that

*   They can be assigned to a variable

*   Passed around as function argument

*   Returned from a function

In GO function is also a type. Two functions are of the same type if they have the same arguments and the same return values. While passing a function as an argument to another function, the exact signature of the function has to be specified in the argument list. As in below example print function accept first argument which is a function of type **func(int, int) int**

```
func print(f func(int, int) int, a, b int)
```

Some more things to note about the below program

*   function **area** is a function of  **func(int, int) int**

*   function **sum** is a function of type **func(int, int) int**

*   **area** and **sum** are of same type as they have same arguments type and same return values type

*   **print** function accepts a function as its first argument of type **func(int, int) int**

*   Thus both **area** and **sum** function can be passed as an argument to the **print** function.

**Code:**

```
package main

import "fmt"

func main() {
    print(area, 2, 4)
    print(sum, 2, 4)
}

func print(f func(int, int) int, a, b int) {
    fmt.Println(f(a, b))
}

func area(a, b int) int {
    return a * b
}

func sum(a, b int) int {
    return a + b
}
```

**Output**

```
8
6
```