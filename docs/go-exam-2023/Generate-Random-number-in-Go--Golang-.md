<!--yml

类别：未分类

日期：2024-10-13 06:15:53

-->

# 在 Go (Golang)中生成随机数

> 来源：[`golangbyexample.com/generate-random-number-golang/`](https://golangbyexample.com/generate-random-number-golang/)

目录

+   概述

+   什么是伪随机数

+   rand 包中的随机生成器函数

    +   具有范围的伪随机数生成器函数。

    +   没有范围的伪随机数生成器函数。

    +   伪随机数生成器函数（适用于浮点数的范围）

+   代码

# **概述**

Go 提供了一个**‘math/rand’**包，内置支持生成伪随机数。此包定义了可以用于生成的各种方法。

+   从 0 到 n 的伪随机数

+   未指定范围的伪随机数。范围将取决于 int 的类型，即 int64、int32、uint64 等。

# **什么是伪随机数**

在继续之前，先理解一下伪随机数的含义。伪随机数并不是真正随机的，因为其值完全由初始值（称为种子）决定。

要理解种子的作用，我们首先看一下可以生成范围为[0, n)的随机数的基本函数。下面的函数在**rand**包中可以用于生成范围为[0, n)的伪随机数。末尾的括号[0,n)表示 n 是排除的。

```go
func Intn(n int) int
```

上述函数返回一个在 0 到 n 之间的 int 值。让我们写一个没有种子值的程序。我们传递了 10，因此下面的函数将在范围[0,10)内生成随机数。

```go
package main

import (
    "fmt"
    "math/rand"
)

func main() {
    fmt.Println(rand.Intn(10))
    fmt.Println(rand.Intn(10))
    fmt.Println(rand.Intn(10))
}
```

尝试多次运行上述程序。每次都会得到相同的输出。在我的系统上，输出如下。

```go
7
7
7
```

现在让我们尝试运行相同的程序，但首先提供种子值。

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())

    fmt.Println(rand.Intn(10))
    fmt.Println(rand.Intn(10))
    fmt.Println(rand.Intn(10))
```

我们将种子值设为自 1970 年 1 月 1 日 UTC 以来经过的秒数。

```go
rand.Seed(time.Now().Unix())
```

每次执行此程序时都会给出不同的输出，因为种子值不同。这就是我们所说的 Go 生成伪随机数的含义。

# **rand 包中的随机生成器函数**

现在我们已经理解了伪随机数生成，让我们看看 rand 包中提供的一些用于生成随机数的函数。你可以根据需求使用这些函数中的任何一个来生成随机数。

## **具有范围的伪随机数生成器函数**。

所有函数将 n 作为参数，如果 n<=0，则会出现恐慌。

+   Intn(n int) – 返回一个非负的伪随机数，范围在[0,n)内

+   Int31n(n int32) – 返回一个非负的伪随机数，范围在[0,n)内，返回类型为 int32

+   Int63n(n int64) – 返回一个非负的伪随机数，范围在[0,n)内，返回类型为 int64

## **无范围的伪随机数生成器函数。**

+   Int() – 返回一个非负的伪随机整数

+   Int31() – 返回一个非负的伪随机 31 位整数，类型为 int32

+   Int63() – 返回一个非负的伪随机 63 位整数，类型为 int64

+   Uint32() – 返回一个伪随机的 32 位值，类型为 uint32

+   Uint64() – 返回一个伪随机的 64 位值，类型为 uint64

## **有范围的伪随机数生成器函数（用于浮点数）**

+   Float64() – 返回一个伪随机数，范围在[0.0,1.0)内，类型为 float64

+   Float32() – 返回一个伪随机数，范围在[0.0,1.0)内，类型为 float32

# **代码**

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())
    //Pseudo Random Number Generator Functions with range

    //1\. Intn(n int)
    fmt.Printf("Intn: %d\n", rand.Intn(10))

    //2\. Int31n(n int32)
    fmt.Printf("Int31n: %d\n", rand.Int31n(10))

    //3\. Int64n(n int32)
    fmt.Printf("Int64n: %d\n", rand.Int63n(10))

    //Pseudo Random Number Generator Functions without range.

    //1\. Int()
    fmt.Printf("Int: %d\n", rand.Int())

    //2\. Int31()
    fmt.Printf("Int31: %d\n", rand.Int31())

    //3\. Int63()
    fmt.Printf("Int63: %d\n", rand.Int63())

    //4\. Uint32()
    fmt.Printf("Uint32: %d\n", rand.Uint32())

    //4\. Uint64()
    fmt.Printf("Uint64: %d\n", rand.Uint64())

    //Pseudo Random Number Generator Functions with range for floats

    //1\. Float64()
    fmt.Printf("Float64: %f\n", rand.Float64())

    //2\. Float32()
    fmt.Printf("Float32: %f\n", rand.Float32())
}
```

**输出：**

```go
Intn: 9
Int31n: 4
Int64n: 4
Int: 6567086139449890598
Int31: 402632083
Int63: 428924242891364663
Uint32: 1991553101
Uint64: 825780166485441015
Float64: 0.683701
Float32: 0.382141
```

+   [生成](https://golangbyexample.com/tag/generate/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [数字](https://golangbyexample.com/tag/numbers/)*   [随机](https://golangbyexample.com/tag/random/)*
