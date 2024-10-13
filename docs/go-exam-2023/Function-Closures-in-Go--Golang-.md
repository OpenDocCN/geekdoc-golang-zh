<!--yml

分类：未分类

date: 2024-10-13 06:10:53

-->

# Go（Golang）中的函数闭包

> 来源：[`golangbyexample.com/function-closures-golang/`](https://golangbyexample.com/function-closures-golang/)

目录

**   概述

+   代码：

    +   示例 1

    +   示例 2：

    +   示例 3：*  *# **概述**

函数闭包只是一个匿名函数，它可以访问在函数外部声明的变量，并且在不同的函数调用之间保留这些变量的当前值。匿名函数是没有名称的函数。

当一个函数在另一个函数内定义时，闭包发生，内层函数可以访问外层函数的变量。

你可以在这里阅读更多关于闭包的内容。

[`en.wikipedia.org/wiki/Closure_(computer_programming)`](https://en.wikipedia.org/wiki/Closure_(computer_programming))

# **代码：**

让我们看一个例子。在示例 1 中有三件事需要注意。

+   getModulus 函数返回一个闭包。它被赋值给变量 **modulus**

+   这个闭包函数可以访问在其主体外定义的 **count** 变量。

+   **count** 变量的值在 modulus 函数的不同调用之间得以保留。

## **示例 1**

```go
package main

import (
    "fmt"
)

func main() {
    modulus := getModulus()
    modulus(-1)
    modulus(2)
    modulus(-5)
}

func getModulus() func(int) int {
    count := 0
    return func(x int) int {
        count = count + 1
        fmt.Printf("modulus function called %d times\n", count)
        if x < 0 {
            x = x * -1
        }
        return x
    }
}
```

**输出：**

```go
modulus function called 1 times
modulus function called 2 times
modulus function called 3 times
```

## **示例 2：**

下面是另一个闭包函数的示例。该函数能够访问 **valueOutside** 变量。

```go
package main

import "fmt"

func main() {
    valueOutside := "somevalue"
    func() {
        fmt.Println(valueOutside)
    }()
}
```

**输出：**

```go
somevalue
```

## **示例 3：**

在下面的示例中，闭包函数能够访问 **count** 变量及其值，并且该值在不同的函数调用之间得以保留。

```go
package main

import "fmt"

func main() {
    count := 0
    for i := 1; i <= 5; i++ {
        func() {
            count++
            fmt.Println(count)
        }()
    }
}
```

**输出**

```go
1
2
3
4
5
```*
