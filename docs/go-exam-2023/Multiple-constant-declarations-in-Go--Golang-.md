<!--yml

分类：未分类

日期：2024-10-13 06:28:14

-->

# Go（Golang）中的多个常量声明

> 来源：[`golangbyexample.com/multiple-constant-declarations-go/`](https://golangbyexample.com/multiple-constant-declarations-go/)

目录

**   概述

+   同时声明多个常量，具有不同的值和类型

+   同时声明多个常量，具有相同的值和类型

+   组合上述两个

+   单行多个声明*  *# **概述**

以下是一些同时声明多个常量的方法。

# **同时声明多个常量，具有不同的值和类型**

```go
const (
  a = "circle"
  b = 1
  c float = 4.65
)
```

声明可以是有类型的或无类型的。请参阅本文了解有类型常量和无类型常量之间的区别 – [`golangbyexample.com/typed-untyped-constant-golang/`](https://golangbyexample.com/typed-untyped-constant-golang/)

+   a 是一个无类型声明。它将是**string**类型，值为**“circle”**。

+   b 也是一个未类型声明。它将是**int**类型，值为**1**。

+   c 是一个有类型的声明。它将是**float64**类型，值为 4.65。

# **同时声明多个常量，具有相同的值和类型**

```go
const (
  a string = "circle"
  b
)
```

当常量的类型和值未提供时，它将从之前的声明中获取其类型和值。

+   a 将是**string**类型，值为**“circle”**。

+   b 将是**string**类型，值为**“circle”**。

# **组合上述两个**

```go
const (
  a string "circle"
  b
  c = 1
)
```

+   a 将是**string**类型，值为**“circle”**。

+   b 将是**string**类型，值为**“circle”**。

+   c 将是**int**类型，值为**1**。

# **单行多个声明**

```go
const a, b = 1, 2
const c, d int = 3, 4
```

声明再次可以是有类型的或无类型的。

+   **a**将是**int**类型，值为**1**。

+   **b**将是**int**类型，值为**2**。

+   **c**将是**int**类型，值为**3**。

+   **d**将是**int**类型，值为**4**。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
