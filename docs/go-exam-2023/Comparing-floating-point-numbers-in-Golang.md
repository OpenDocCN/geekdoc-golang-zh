<!--yml

类别：未分类

日期：2024-10-13 06:05:52

-->

# 在Golang中比较浮点数

> 来源：[https://golangbyexample.com/comparing-floating-point-numbers-go/](https://golangbyexample.com/comparing-floating-point-numbers-go/)

目录

**   [引言](#Introduction "Introduction")

+   [比较相同浮点类型](#Comparing_Same_Float_Types "Comparing Same Float Types")

+   [比较不同浮点类型](#Comparing_Different_Float_Types "Comparing Different Float Types")

+   [比较相同浮点类型的容差](#Comparing_Same_Float_Types_With_Tolerance "Comparing Same Float Types With Tolerance")

+   [使用math.big包进行比较](#Comparing_Using_mathbig_package "Comparing Using math.big package")*  *# 引言

Go语言有两种浮点类型

+   float32

+   float64

# **比较相同浮点类型**

可以使用Go的==运算符比较两个浮点数，前提是它们是相同的浮点类型。请看下面的例子

```go
package main

import "fmt"

func main() {
    a := 3.14
    b := 3.14
    if a == b {
        fmt.Println("Same")
    } else {
        fmt.Println("Not Same")
    }

    a = 3.142
    b = 3.14
    if a == b {
        fmt.Println("Same")
    } else {
        fmt.Println("Not Same")
    }
}
```

**输出：**

```go
Same
Not Same
```

# **比较不同浮点类型**

比较float32和float64将导致编译错误

```go
package main

import "fmt"

func main() {

    var a float32
    var b float64
    if a == b {
        fmt.Println("Same")
    } else {
        fmt.Println("Not Same")
    }
}
```

**输出：**

```go
invalid operation: a == b (mismatched types float32 and float64)
```

# **比较相同浮点类型的容差**

如果在比较浮点类型时可接受某种容差，则可以使用以下方法

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    withTolerane(3.14, 3.141)
    withTolerane(3.14, 3.142)
}

func withTolerane(a, b float64) {
    tolerance := 0.001
    if diff := math.Abs(a - b); diff < tolerance {
        fmt.Printf("When a=%f and b =%f => Nearly same by tolerance\n", a, b)
    } else {
        fmt.Printf("When a=%f and b=%f => Not same Even by Tolerance\n", a, b)
    }
}
```

**输出：**

```go
When a=3.140000 and b =3.141000 => Nearly same by tolerance
When a=3.140000 and b=3.142000 => Not same Even by Tolerance
```

# **使用math.big包进行比较**

big包支持大数，并支持整型、有理数和浮点数。它有一个比较方法，可以用来比较两个浮点数

```go
package main

import (
    "fmt"
    "math/big"
)

func main() {
    a := 3.1432
    b := 3.1456
    // compare a to b
    result := big.NewFloat(a).Cmp(big.NewFloat(b))

    // -1 if a < b
    if result < 0 {
        fmt.Println("a less than b")
    }

    // 0 if a == b
    if result == 0 {
        fmt.Println("a  equals to b")
    }

    // +1 if a > b
    if result > 0 {
        fmt.Println("a 1 more than b")
    }
}
```

**输出：**

```go
a less than b
```

+   [比较](https://golangbyexample.com/tag/compare/)*   [浮点](https://golangbyexample.com/tag/float/)*   [Go中的浮点数](https://golangbyexample.com/tag/floating-point-numbers-in-go/)*
