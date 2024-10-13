<!--yml

类别：未分类

日期：2024-10-13 06:11:02

-->

# 在 Go (Golang) 中从函数返回一个函数

> 来源：[https://golangbyexample.com/return-func-from-func-go/](https://golangbyexample.com/return-func-from-func-go/)

在 Golang 中，函数是一级变量，这意味着

+   它们可以被赋值给一个变量

+   作为函数参数传递

+   从函数返回

在从另一个函数返回一个函数时，必须在返回列表中指定函数的确切签名。如下例所示

+   **getAreaFunc** 函数的返回类型是 **func(int, int) int**

```go
func getAreaFunc() func(int, int)
```

+   **getAreaFunc** 函数因此可以返回类型为 **func(int, int) int** 的函数

**代码：**

```go
package main

import "fmt"

func main() {
    areaF := getAreaFunc()
    res := areaF(2, 4)
    fmt.Println(res)
}

func getAreaFunc() func(int, int) int {
    return func(x, y int) int {
        return x * y
    }
}
```

**输出：**

```go
8
```
