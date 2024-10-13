<!--yml

category: 未分类

date: 2024-10-13 06:15:11

-->

# 在 Go (Golang) 中的平方根

> 来源：[`golangbyexample.com/square-root-number-golang/`](https://golangbyexample.com/square-root-number-golang/)

目录

+   概述

+   代码：

# **概述**

GO 的**math**包提供了一个**Sqrt**方法，可以用来获取该数字的立方根。

以下是该函数的签名。它接受一个浮点数输入，并返回一个浮点数。

```go
func Sqrt(x float64) float64
```

Sqrt 函数的一些特殊情况是

+   Sqrt(±0) = ±0

+   Sqrt(±Inf) = ±Inf

+   Sqrt(x < 0) = NaN

+   Sqrt(NaN) = NaN

# **代码：**

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	//Square root of a integer
	res := math.Sqrt(4)
	fmt.Println(res)

	//Square root of a integer
	res = math.Sqrt(9)
	fmt.Println(res)

	//Square Root of a float
	res = math.Sqrt(30.33)
	fmt.Println(res)

	//Square Root of a negative number
	res = math.Sqrt(-9)
	fmt.Println(res)
} 
```

**输出：**

```go
2
3
5.5072679252057455
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [平方根](https://golangbyexample.com/tag/square-root/)*
