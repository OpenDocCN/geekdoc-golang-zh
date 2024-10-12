<!--yml
category: 未分类
date: 2024-10-13 06:11:19
-->

# User defined function types in Go (Golang)

> 来源：[https://golangbyexample.com/user-defined-function-type-go/](https://golangbyexample.com/user-defined-function-type-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")
    *   [Example 1](#Example_1 "Example 1")
    *   [Example 2](#Example_2 "Example 2")*  *# **Overview**

In GO, the function is also a type. Two functions will be of the same type if

*   They have the same number of arguments with each argument is of the same type

*   They have the same number of return values and each return value is of the same type

Function as user-defined type can be declared using the **type** keyword like below. **area** is the name of the function of type **func(int, int) int**

```
type area func(int, int) int
```

# **Code**

## **Example 1**

In this example, we create a user-defined function type area. Then we create a variable of type area in the main function.

```
package main

import "fmt"

type area func(int, int) int

func main() {
    var areaF area = func(a, b int) int {
        return a * b
    }
    print(2, 3, areaF)
}

func print(x, y int, a area) {
    fmt.Printf("Area is: %d\n", a(x, y))
}
```

**Output:**

```
6
```

## **Example 2**

In this example also we create a user-defined function type **area.** Then we create a function **getAreaFunc()** which returns the function of type **area**

```
package main

import "fmt"

type area func(int, int) int

func main() {
    areaF := getAreaFunc()
    print(2, 3, areaF)

}

func print(x, y int, a area) {
    fmt.Printf("Area is: %d\n", a(x, y))
}

func getAreaFunc() area {
    return func(x, y int) int {
        return x * y
    }
}
```

**Output:**

```
6
```

*   [golang](https://golangbyexample.com/tag/golang/)*