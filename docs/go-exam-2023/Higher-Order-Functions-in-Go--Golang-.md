<!--yml

分类：未分类

日期：2024-10-13 06:11:15

-->

# Go（Golang）中的高阶函数

> 来源：[`golangbyexample.com/gohigher-order-functions/`](https://golangbyexample.com/gohigher-order-functions/)

目录

**   概述

+   代码：

    +   示例 1

    +   示例 2：*  *# **概述**

高阶函数是指那些接受一个函数作为参数或返回一个函数的函数。由于在 Golang 中函数是一个一阶变量，它们可以被传递、返回，并赋值给一个变量。

# **代码：**

在下面的示例 1 中

+   **print** 函数接受一个类型为 **func(int, int) int** 的函数作为参数

+   **getAreafunc** 返回一个类型为 **func(int, int) int** 的函数

## **示例 1**

```go
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

**输出：**

```go
12
```

## **示例 2：**

我们再来看一个稍微复杂的例子，其中

+   两个函数作为参数传递

+   一个函数返回两个函数

```go
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

**输出**：

```go
Sum is: 7
Difference Value is: 1
```*
