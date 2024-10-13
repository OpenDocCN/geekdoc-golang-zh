<!--yml

分类：未分类

日期：2024-10-13 06:15:32

-->

# 计算x^y – Go中的Pow()

> 来源：[https://golangbyexample.com/power-golang/](https://golangbyexample.com/power-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code") *  *# **概述**

GO的**math**包提供了一个**Pow**方法，可以用来计算x的y次幂。

以下是该函数的签名。它接受两个浮点参数作为输入，并返回一个浮点数。

```go
func Pow(x, y float64) float64
```

同样的函数也可以用来计算一个数字的平方或立方。只需在**平方**情况下将第二个参数y传入2，在**立方**情况下传入3即可。

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

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *   [power](https://golangbyexample.com/tag/power/) *
