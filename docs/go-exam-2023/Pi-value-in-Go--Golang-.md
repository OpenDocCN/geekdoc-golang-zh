<!--yml

分类：未分类

日期：2024-10-13 06:15:01

-->

# Go（Golang）中的圆周率值

> 来源：[`golangbyexample.com/pi-value-golang/`](https://golangbyexample.com/pi-value-golang/)

目录

+   概述

+   代码：

# **概述**

GO 的**math**包提供了**圆周率**常量。

它在 math 包的 constant.go 文件中是这样定义的。它是一个 float64 值。

```go
Pi  = 3.14159265358979323846264338327950288419716939937510582097494459
```

# **代码：**

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	pi := math.Pi
	fmt.Println(pi)
} 
```

**输出：**

```go
3.141592653589793
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [math](https://golangbyexample.com/tag/math/)*   [pi](https://golangbyexample.com/tag/pi/)*
