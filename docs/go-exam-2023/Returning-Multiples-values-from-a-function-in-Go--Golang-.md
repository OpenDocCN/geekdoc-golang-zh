<!--yml

类别：未分类

日期：2024-10-13 06:11:24

-->

# 在 Go (Golang) 中从函数返回多个值

> 来源：[`golangbyexample.com/go-return-multiples-values-function/`](https://golangbyexample.com/go-return-multiples-values-function/)

目录

+   概述

+   函数的签名

+   返回值

    +   代码：

+   命名返回值

    +   代码：

# **概述**

函数是一组执行特定任务的语句。

# **函数的签名**

```go
func func_name(input_parameters) return_values{
  //body
}
```

在 Golang 中，一个函数可以返回多个值。下面是一个函数的示例。

+   名称为 **“f”**

+   接受两个整型参数

+   返回一个整型的单个值

```go
func f(a int, b int) int {
  return a + b 
}
```

# **返回值**

+   如上所述，一个函数可以有一个或多个返回值。假设有一个函数 **sum_avg** 返回两个值：总和和平均值。注意，当返回多个值时，返回值类型必须用括号括起来。多个返回值的示例。

```go
func sum_avg(a, b int) (int, int)
```

+   根据约定，错误作为函数的最后一个参数返回。示例

```go
func sum(a, b int) (int, error)
```

+   在调用函数中收集多个返回值。在下面的示例中

```go
result, err := sum(2, 3) 
```

## **代码：**

```go
package main

import "fmt"

func main() {
    sum, avg := sum_avg(4, 2)
    fmt.Println(sum)
    fmt.Println(avg)
}

func sum_avg(a, b int) (int, int) {
    sum := a + b
    avg := (a + b) / 2
    return sum, avg
}
```

**输出**：

```go
6
3
```

# **命名返回值**

Go 函数可以具有命名返回值。使用命名返回值时，返回值在函数中不需要初始化。命名变量在签名中指定。没有命名值时，只指定返回类型。也可以为某些返回值命名。对于其他返回值，只能指定类型。

+   见下面的示例：**result** 是命名返回值。

```go
func sum(a, b int) (result int)
```

+   多个命名返回值

```go
func sum_avg(a, b int) (sum int, avg int)
```

+   使用命名返回值时，相同类型的连续值只需指定一次类型。

```go
func sum_avg(a, b int) (sum, avg int)
```

+   命名返回值被初始化为该类型的零值。因此我们不需要在函数中重新初始化它。在下面的示例中，**sum** 和 **avg** 没有使用 **:=** 符号再次初始化。

## **代码：**

```go
package main

import "fmt"

func main() {
    sum, avg := sum_avg(4, 2)
    fmt.Println(sum)
    fmt.Println(avg)
}

func sum_avg(a, b int) (sum, avg int) {
    sum = a + b
    avg = (a + b) / 2
    return
} 
```

**输出**

```go
6
3
```*
