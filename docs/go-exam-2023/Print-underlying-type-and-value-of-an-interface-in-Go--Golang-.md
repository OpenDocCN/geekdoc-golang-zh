<!--yml

分类：未分类

日期：2024-10-13 06:22:41

-->

# 在 Go（Golang）中打印接口的基础类型和值

> 来源：[`golangbyexample.com/print-type-value-interface-golang/`](https://golangbyexample.com/print-type-value-interface-golang/)

目录

+   概述

+   代码

# **概述**

Golang 提供格式标识符来打印接口值所表示的基础类型和基础值。

+   %T 可以用来打印接口值的具体类型

+   %v 可以用来打印接口值的具体值。

假设我们有一个接口**动物**如下

```go
type animal interface {
    breathe()
    walk()
}
```

我们还有一个**狮子**结构体实现了这个**动物**接口。

```go
type lion struct {
    age int
}
```

# **代码**

```go
package main

import "fmt"

type animal interface {
    breathe()
    walk()
}

type lion struct {
    age int
}

func (l lion) breathe() {
    fmt.Println("Lion breathes")
}

func (l lion) walk() {
    fmt.Println("Lion walk")
}

func main() {
    var a animal
    a = lion{age: 10}
    fmt.Printf("Underlying Type: %T\n", a)
    fmt.Printf("Underlying Value: %v\n", a)
}
```

**输出**

```go
Concrete Type: main.lion
Concrete Value: {10}
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
