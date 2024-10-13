<!--yml

类别：未分类

日期：2024-10-13 06:34:54

-->

# 在 Go（Golang）中将浮点数转换为整数

> 来源：[`golangbyexample.com/float-to-int-golang/`](https://golangbyexample.com/float-to-int-golang/)

目录

+   概述

+   float64 转 int

+   float32 转 int

## **概述**

Golang 需要明确转换才能在不同类型之间进行转换。浮点数据类型可以通过显式类型转换直接转换为浮点数据类型。以下是其语法。

```go
{destination_type}(some_value) 
```

这将**some_value**转换为**目标**类型。

## **float64 转 int**

```go
var a float64 = 12
var b int = int(a)
```

或

```go
b := int(a)
```

以下是相应的程序

```go
package main

import "fmt"

func main() {
    var a float64 = 12
    var b int = int(a)
    fmt.Printf("Underlying Type of b: %T\n", b)

    b2 := int(a)
    fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

**输出**

```go
Underlying Type of b: int
Underlying Type of b2: int
```

## **float32 转 int**

```go
var a float32 = 12
var b int = int(a)
```

或

```go
b := int(a)
```

以下是相应的工作程序。

```go
package main

import "fmt"

func main() {
    var a float32 = 12
    var b int = int(a)
    fmt.Printf("Underlying Type of b: %T\n", b)

    b2 := int(a)
    fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

**输出**

```go
Underlying Type of b: int
Underlying Type of b2: int
```

如果我们直接将浮点值赋给整数变量而没有明确转换，将会引发以下编译错误。

```go
cannot use a (type float64) as type int in assignment
```

或

```go
cannot use a (type float32) as type int in assignment
```

另外，请查看我们的 Golang 进阶教程系列 – [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
