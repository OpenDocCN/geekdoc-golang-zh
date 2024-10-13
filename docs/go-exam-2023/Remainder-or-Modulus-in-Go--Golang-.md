<!--yml

类别：未分类

日期：2024-10-13 06:15:22

-->

# Go（Golang）中的余数或模运算

> 来源：[`golangbyexample.com/remainder-modulus-go-golang/`](https://golangbyexample.com/remainder-modulus-go-golang/)

目录

+   概述

+   % 运算符

    +   代码

+   浮点数的 Mod 函数

    +   代码

+   IEEE 754 余数

    +   代码

# **概述**

在本教程中，我们将学习关于

+   **% 运算符** – 适用于获取整数的余数

+   **Mod 函数** – 也可以用于获取浮点数的余数

+   **余数函数** – 可用于获取 IEEE 754 余数

# **% 运算符**

Golang 有一个模运算符 **(‘ %’)**，可用于获取两个整数相除后的余数。我们来看一个示例程序。

## **代码**

```go
package main

import (
    "fmt"
)

func main() {
    res := 4 % 2
    fmt.Println(res)

    res = 5 % 2
    fmt.Println(res)

    res = 8 % 3
    fmt.Println(res)
}
```

**输出：**

```go
0
1
2
```

# **浮点数的 Mod 函数**

% 函数不适用于浮点数。要获取两个浮点数相除的余数，我们可以使用 **math** 包提供的 **Mod** 函数。以下是该函数的签名。它接受两个浮点数并返回一个浮点数，返回 x/y 的浮点余数，输出将采用 x 的符号。

```go
func Mod(x, y float64) float64
```

**Mod** 函数的一些特殊情况是

+   Mod(±Inf, y) = NaN

+   Mod(NaN, y) = NaN

+   Mod(x, 0) = NaN

+   Mod(x, ±Inf) = x

+   Mod(x, NaN) = NaN

## **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Mod(4, 2)
    fmt.Println(res)

    res = math.Mod(4.2, 2)
    fmt.Println(res)

    res = math.Mod(5, 2)
    fmt.Println(res)

    res = math.Mod(-5, 2)
    fmt.Println(res)
}
```

**输出**

```go
0
0.20000000000000018
1
-1
```

# **IEEE 754 余数**

Go 的 **math** 包提供了一个 **Remainder** 方法，可以用来获取两个数的 **IEEE 754 余数**，其中一个作为分子，另一个作为分母。

你可以在这里阅读更多关于我们为什么需要 **IEEE 754 余数** 的信息 – [`stackoverflow.com/questions/26671975/why-do-we-need-ieee-754-remainder`](https://stackoverflow.com/questions/26671975/why-do-we-need-ieee-754-remainder)

以下是该函数的签名。它接受两个 float64 类型的数字并返回一个余数，这也是一个 **IEEE 754 float64 余数**。

```go
func Remainder(x, y float64) float64
```

余数函数的一些特殊情况是

+   Remainder(±Inf, y) = NaN

+   Remainder(NaN, y) = NaN

+   Remainder(x, 0) = NaN

+   Remainder(x, ±Inf) = x

+   Remainder(x, NaN) = NaN

## **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Remainder(4, 2)
    fmt.Println(res)

    res = math.Remainder(5, 2)
    fmt.Println(res)

    res = math.Remainder(5.5, 2)
    fmt.Println(res)

    res = math.Remainder(5.5, 1.5)
    fmt.Println(res)
}
```

**输出**

```go
0
1
-0.5
-0.5
```


