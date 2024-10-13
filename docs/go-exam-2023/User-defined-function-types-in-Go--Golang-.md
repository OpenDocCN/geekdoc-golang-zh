<!--yml

分类：未分类

日期：2024-10-13 06:11:19

-->

# Go (Golang)中的用户定义函数类型

> 来源：[https://golangbyexample.com/user-defined-function-type-go/](https://golangbyexample.com/user-defined-function-type-go/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")

    +   [示例 1](#Example_1 "Example 1")

    +   [示例 2](#Example_2 "Example 2")*  *# **概述**

在GO中，函数也是一种类型。如果两个函数是同一类型的话。

+   它们具有相同数量的参数，每个参数都是相同类型的。

+   它们具有相同数量的返回值，并且每个返回值都是相同类型的。

可以使用**type**关键字声明用户定义的类型作为函数，如下所示。**area**是类型为**func(int, int) int**的函数名称。

```go
type area func(int, int) int
```

# **代码**

## **示例 1**

在这个例子中，我们创建一个用户定义的函数类型**area**。然后在主函数中创建一个类型为**area**的变量。

```go
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

**输出：**

```go
6
```

## **示例 2**

在这个例子中，我们也创建一个用户定义的函数类型**area**。然后我们创建一个函数**getAreaFunc()**，它返回类型为**area**的函数。

```go
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

**输出：**

```go
6
```

+   [golang](https://golangbyexample.com/tag/golang/)*
