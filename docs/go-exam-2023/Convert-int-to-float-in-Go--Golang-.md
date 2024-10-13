<!--yml

类别：未分类

日期：2024-10-13 06:35:00

-->

# 在Go中将int转换为float（Golang）

> 来源：[https://golangbyexample.com/int-to-float-golang/](https://golangbyexample.com/int-to-float-golang/)

目录

**   [概述](#Overview "Overview")

+   [int到float64](#int_to_float64 "int to float64")

+   [int到float32](#int_to_float32 "int to float32")*  *## **概述**

Golang需要显式转换才能从一种类型转换为另一种类型。int数据类型可以通过显式类型转换直接转换为float数据类型。以下是语法。

```
{destination_type}(some_value) 
```

这将**some_value**转换为**destination_type**。

## **int到float64**

```
var a int = 12
var b float64 = float64(a)
```

或

```
b := float64(a)
```

以下是相同的程序

```
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

```
Underlying Type of b: float64
Underlying Type of b2: float64
```

## **int到float32**

```
var a int = 12
var b float32 = float32(a)
```

或

```
b := float32(a)
```

以下是相同的工作程序。

```
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

```
Underlying Type of b: float32
Underlying Type of b2: float32
```

如果我们直接将一个int赋值给float变量而不进行转换，将会引发编译错误。

```
cannot use a (type int) as type float64 in assignment
```

另外，查看我们的Golang高级教程系列 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
