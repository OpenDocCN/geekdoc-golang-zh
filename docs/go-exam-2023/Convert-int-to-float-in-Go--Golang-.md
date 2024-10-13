<!--yml

类别：未分类

日期：2024-10-13 06:35:00

-->

# 在 Go 中将 int 转换为 float（Golang）

> 来源：[`golangbyexample.com/int-to-float-golang/`](https://golangbyexample.com/int-to-float-golang/)

目录

**   概述

+   int 到 float64

+   int 到 float32*  *## **概述**

Golang 需要显式转换才能从一种类型转换为另一种类型。int 数据类型可以通过显式类型转换直接转换为 float 数据类型。以下是语法。

```go
{destination_type}(some_value) 
```

这将**some_value**转换为**destination_type**。

## **int 到 float64**

```go
var a int = 12
var b float64 = float64(a)
```

或

```go
b := float64(a)
```

以下是相同的程序

```go
package main
import "fmt"
func main() {
    var a int = 12
    var b float32 = float64(a)
    fmt.Printf("Underlying Type of b: %T\n", b)

    b2 := float64(a)
    fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

**输出**

```go
Underlying Type of b: float64
Underlying Type of b2: float64
```

## **int 到 float32**

```go
var a int = 12
var b float32 = float32(a)
```

或

```go
b := float32(a)
```

以下是相同的工作程序。

```go
package main

import "fmt"

func main() {
	var a int = 12
	var b float32 = float32(a)

	fmt.Printf("Underlying Type of b: %T\n", b)

	b2 := float32(a)
	fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

**输出**

```go
Underlying Type of b: float32
Underlying Type of b2: float32
```

如果我们直接将一个 int 赋值给 float 变量而不进行转换，将会引发编译错误。

```go
cannot use a (type int) as type float64 in assignment
```

另外，查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
