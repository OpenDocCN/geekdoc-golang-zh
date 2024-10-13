<!--yml

分类：未分类。

日期：2024-10-13 06:10:57

-->

# 在 Go（Golang）中将函数作为参数传递给另一个函数。

> 来源：[https://golangbyexample.com/func-as-func-argument-go/](https://golangbyexample.com/func-as-func-argument-go/)

Golang 函数是一级变量，意味着。

+   它们可以被赋值给一个变量。

+   作为函数参数传递。

+   从函数返回。

在 Go 中，函数也是一种类型。如果两个函数具有相同的参数和相同的返回值，它们就是同一种类型。在将一个函数作为参数传递给另一个函数时，必须在参数列表中指定函数的确切签名。如下例所示，print 函数接受的第一个参数是类型为 **func(int, int) int** 的函数。

```go
func print(f func(int, int) int, a, b int)
```

关于下面程序还有一些需要注意的事项。

+   函数 **area** 是类型为 **func(int, int) int** 的函数。

+   函数 **sum** 是类型为 **func(int, int) int** 的函数。

+   **area** 和 **sum** 是同一种类型，因为它们具有相同的参数类型和相同的返回值类型。

+   **print** 函数接受一个类型为 **func(int, int) int** 的函数作为其第一个参数。

+   因此，**area** 和 **sum** 函数可以作为参数传递给 **print** 函数。

**代码：**

```go
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

**输出**

```go
8
6
```
