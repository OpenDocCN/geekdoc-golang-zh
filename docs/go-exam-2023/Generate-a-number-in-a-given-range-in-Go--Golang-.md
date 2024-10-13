<!--yml

类别：未分类

日期：2024-10-13 06:16:40

-->

# 在Go（Golang）中生成给定范围内的数字

> 来源：[https://golangbyexample.com/random-number-range-golang/](https://golangbyexample.com/random-number-range-golang/)

目录

**   [概述](#Overview "概述")**

+   [代码](#Code "代码")*  *# **概述**

**‘mat/rand’** 包含一个 **Intn** 函数，可以用来生成一个在[0,n)之间的随机数。末尾的括号表示n是排除的。

要了解更多关于伪随机数的含义，请查看这篇文章 – [https://golangbyexample.com/generate-random-number-golang](https://golangbyexample.com/generate-random-number-golang)

下面是此方法的签名。它接受一个数字n并返回一个范围在0<=x<n的数字。

```
func Intn(n int) int
```

上述函数也可以用来生成范围在a到b之间的随机数。请看下面的程序。它用于生成一个在范围a到b之间的数字。我们还提供了一个种子值给**rand**，以便每次生成不同的输出。

# **代码**

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    //Generate a random number x where x is in range 5<=x<=20
    rangeLower := 5
    rangeUpper := 20
    randomNum := rangeLower + rand.Intn(rangeUpper-rangeLower+1)
    fmt.Println(randomNum)

    //Generate a random number x where x is in range 100<=x<=200
    rangeLower = 100
    rangeUpper = 200
    randomNum = rangeLower + rand.Intn(rangeUpper-rangeLower+1)
    fmt.Println(randomNum)
}
```

**输出：**

```
Number between 5<=x<=20
Number between 100<=x<=200
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [随机](https://golangbyexample.com/tag/random/)*
