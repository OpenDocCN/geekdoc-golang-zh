<!--yml

类别：未分类

日期：2024-10-13 06:14:45

-->

# 在 Go (Golang) 中进行偶数舍入

> 来源：[https://golangbyexample.com/round-even-number-golang/](https://golangbyexample.com/round-even-number-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

GO 的 **math** 包提供了一个 **RoundToEven** 方法，可以用来将数字舍入为偶数。它返回最近的整数，并将平局情况舍入为偶数。

在这里你可以阅读关于 RoundToEven 的用例 – [https://mathematica.stackexchange.com/questions/2116/why-round-to-even-integers/2120](https://mathematica.stackexchange.com/questions/2116/why-round-to-even-integers/2120)

下面是该函数的签名。它接受一个浮点数作为输入，并返回一个浮点数。

```
func RoundToEven(x float64) float64
```

RoundToEven 函数的一些特殊情况是

+   RoundToEven(±0) = ±0

+   RoundToEven(±Inf) = ±Inf

+   RoundToEven(NaN) = NaN

# **代码：**

```
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.RoundToEven(0.5)
    fmt.Println(res)

    res = math.RoundToEven(1.5)
    fmt.Println(res)

    res = math.RoundToEven(2.5)
    fmt.Println(res)

    res = math.RoundToEven(3.5)
    fmt.Println(res)

    res = math.RoundToEven(4.5)
    fmt.Println(res)
}
```

**输出：**

```
0
2
2
4
4
```

+   [go](https://golangbyexample.com/tag/go/)*   [math](https://golangbyexample.com/tag/math/)*   [round even](https://golangbyexample.com/tag/round-even/)*
