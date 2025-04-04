<!--yml

分类：未分类

日期：2024-10-13 06:15:32

-->

# 计算 x^y – Go 中的 Pow()

> 来源：[`golangbyexample.com/power-golang/`](https://golangbyexample.com/power-golang/)

目录

+   概述

+   代码 

# **概述**

GO 的**math**包提供了一个**Pow**方法，可以用来计算 x 的 y 次幂。

以下是该函数的签名。它接受两个浮点参数作为输入，并返回一个浮点数。

```go
func Pow(x, y float64) float64
```

同样的函数也可以用来计算一个数字的平方或立方。只需在**平方**情况下将第二个参数 y 传入 2，在**立方**情况下传入 3 即可。

# **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    //Power for integers
    res := math.Pow(2, 10)
    fmt.Println(res)

    //Power for float
    res = math.Pow(1.5, 2)
    fmt.Println(res)

    //Anything to power 0 is 1
    res = math.Pow(3, 0)
    fmt.Println(res)
}
```

**输出：**

```go
1024
2.25
1
```


