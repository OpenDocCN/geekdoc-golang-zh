<!--yml

类别：未分类

日期：2024-10-13 06:15:17

-->

# Go 语言中的数字对数

> 来源：[`golangbyexample.com/log-of-number-go-golang/`](https://golangbyexample.com/log-of-number-go-golang/)

目录

**概述**

+   自然对数

+   代码

+   二进制指数对数（log e）

    +   代码

+   二进制对数（log 2）

    +   代码

+   十进制对数（log 10）

    +   代码*  *# **概述**

在本教程中，我们将看到三种可能的对数类型

+   自然对数

+   二进制指数对数（log e）

+   二进制对数（log 2）

+   十进制对数（log 10）

# 自然对数

GO 的**math**包提供了一个**Log**方法，可用于获取一个数字的自然对数

以下是函数的签名。它的输入为**float64**数字，并返回一个**float64**。

```go
func Log(x float64) float64
```

另外，**Logb**函数的一些特殊情况是

+   Log(±Inf) = +Inf

+   Log(0) = -Inf

+   Log(NaN) = NaN

+   Log(x < 0) = NaN

# **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Log(4)
    fmt.Println(res)

    res = math.Log(10.2)
    fmt.Println(res)

    res = math.Log(-10)
    fmt.Println(res)
}
```

**输出：**

```go
1.3862943611198906
2.322387720290225
NaN
```

# **二进制指数对数（log e）**

golang 的**math**包提供了一个**Logb**方法，可用于获取一个数字的二进制指数

以下是函数的签名。它的输入为**float64**数字，并返回一个**float64**。

```go
func Logb(x float64) float64
```

另外，**Logb**函数的一些特殊情况是

+   Logb(±Inf) = +Inf

+   Logb(0) = -Inf

+   Logb(NaN) = NaN

## **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Logb(4)
    fmt.Println(res)

    res = math.Logb(10.2)
    fmt.Println(res)

    res = math.Logb(-10)
    fmt.Println(res)
}
```

**输出：**

```go
2
3
3
```

# **二进制对数（log 2）**

golang 的**math**包提供了一个**Log2**方法，可用于获取一个数字的二进制对数或以 2 为底的对数

以下是函数的签名。它的输入为 float64 数字，并返回一个 float64。

另外，**Log2**函数的一些特殊情况是

+   **Log2**(±Inf) = +Inf

+   **Log2**(0) = -Inf

+   **Log2**(NaN) = NaN

+   **Log2**(x < 0) = NaN

## **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Log2(4)
    fmt.Println(res)

    res = math.Log2(10.2)
    fmt.Println(res)

    res = math.Log2(-10)
    fmt.Println(res)
}
```

**输出：**

```go
2
3.321928094887362
NaN
```

# **十进制对数（log 10）**

Go 的**math**包提供了一个**Log10**方法，可用于获取一个数字的十进制对数或以 10 为底的对数

以下是函数的签名。它的输入为 float64 数字，并返回一个 float64。

```go
func Log10(x float64) float64
```

另外，**Log10**函数的一些特殊情况是

+   Log10(±Inf) = +Inf

+   Log10(0) = -Inf

+   Log10(NaN) = NaN

+   Log10(x < 0) = NaN

## **代码**

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    res := math.Log10(100)
    fmt.Println(res)

    res = math.Log10(10)
    fmt.Println(res)

    res = math.Log10(-10)
    fmt.Println(res)
}
```

**输出：**

```go
2
1
NaN
```

+   [go](https://golangbyexample.com/tag/go/)*   [log](https://golangbyexample.com/tag/log/)*   [log 10](https://golangbyexample.com/tag/log-10/)*   [log 2](https://golangbyexample.com/tag/log-2/)*   [log e](https://golangbyexample.com/tag/log-e/)*
