<!--yml

category: 未分类

日期: 2024-10-13 06:35:05

-->

# Go（Golang）中的float32与float64之间的转换

> 来源：[https://golangbyexample.com/conversion-float-golang/](https://golangbyexample.com/conversion-float-golang/)

目录

**   [概述](#Overview "概述")

+   [float32 转 float64](#float32_to_float64 "float32 转 float64")

+   [float64 转 float32](#float64_to_float32 "float64 转 float32")*  *## **概述**

Golang需要显式转换才能在两种类型之间转换。float32和float64数据类型之间的转换需要显式类型转换。下面是语法。

```go
{destination_type}(some_value) 
```

这将 **some_value** 转换为 **destination_type**。

## **float32 转 float64**

```go
var a float32 = 12
var b float64 = float64(a)
```

或

```go
b := float64(a)
```

下面是相应的工作程序

```go
package main
import "fmt"
func main() {
    var a float32 = 12.0
    var b float64 = float64(a)
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

## **float64 转 float32**

```go
var a float64 = 12
var b float32 = float32(a)
```

或

```go
b := float32(a)
```

下面是相应的工作程序。

```go
package main

import "fmt"

func main() {
	var a float64 = 12.0
	var b float32 = float32(a)

	fmt.Printf("Underlying Type of b: %T\n", b)

	b2 := float32(a)
	fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

如果我们直接将float64值赋给float32，或反之，且没有明确转换，将会引发编译错误。

```go
cannot use a (type float64) as type float32 in assignment
```

同时，查看我们的Golang进阶教程系列 – [Golang进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
