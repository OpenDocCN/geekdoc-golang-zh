<!--yml

类别：未分类

日期：2024-10-13 06:15:27

-->

# 在 Go 语言中将数字拆分为整数和小数部分

> 来源：[`golangbyexample.com/break-integer-fraction-part-go/`](https://golangbyexample.com/break-integer-fraction-part-go/)

目录

**   概述

+   代码*  *# **概述**

GO 的**math**包提供了一个**Modf**方法，可以用来将浮点数拆分为整数部分和浮动部分。请注意，该函数也将整数部分作为浮点数返回。

以下是该函数的签名。它接受一个浮点数作为输入并返回两个 float64。第一个是整数部分，第二个是小数部分。

```go
func Modf(f float64) (int float64, frac float64)
```

关于上述函数的一些注意事项

+   返回值**int**和**frac**加起来等于输入**f**

+   **int**和**frac**与输入**f**具有相同的符号

另外，**Modf**函数的一些特殊情况是

+   Modf(±Inf) = ±Inf, NaN

+   Modf(NaN) = NaN, NaN

# **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    //Contain both integer and fraction
    integer, fraction := math.Modf(2.5)
    fmt.Printf("Integer: %f. Fraction: %f\n", integer, fraction)

    //Contain only integer part
    integer, fraction = math.Modf(2)
    fmt.Printf("Integer: %f. Fraction: %f\n", integer, fraction)

    //Negative floating point number
    integer, fraction = math.Modf(-2.5)
    fmt.Printf("Integer: %f. Fraction: %f\n", integer, fraction)

    //Contains only fraction part
    integer, fraction = math.Modf(0.5)
    fmt.Printf("Integer: %f. Fraction: %f\n", integer, fraction)
}
```

**输出：**

```go
Integer: 2.000000\. Fraction: 0.500000
Integer: 2.000000\. Fraction: 0.000000
Integer: -2.000000\. Fraction: -0.500000
Integer: 0.000000\. Fraction: 0.500000
```

+   [go](https://golangbyexample.com/tag/go/)*   [integer](https://golangbyexample.com/tag/integer/)*   [math](https://golangbyexample.com/tag/math/)*
