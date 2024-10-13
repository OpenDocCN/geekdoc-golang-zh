<!--yml

类别：未分类

日期：2024-10-13 06:39:45

-->

# Go (Golang) 中的结构体切片

> 来源：[`golangbyexample.com/slice-struct-golang/`](https://golangbyexample.com/slice-struct-golang/)

目录

**   概述

+   程序*  *## **概述**

在 Golang 中创建结构体切片是可能的。实际上，可以为 Go 中的任何数据类型创建切片。下面是创建结构体切片的简单示例

## **程序**

```go
package main

import "fmt"

type employee struct {
	name string
	age  int
}

func main() {
	employees := make([]employee, 3)

	employees[0] = employee{name: "John", age: 21}
	employees[1] = employee{name: "Simon", age: 25}
	employees[2] = employee{name: "David", age: 18}

	for _, e := range employees {
		fmt.Println(e)
	}
}
```

**输出**

```go
{John 21}
{Simon 25}
{David 18}
```

在上述程序中，我们创建了一个名为 **employee** 的结构体

```go
type employee struct {
	name string
	age  int
}
```

然后我们创建了一个结构体的切片，如下所示

```go
employees := make([]employee, 3)
```

这就是我们如何创建结构体切片

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力涵盖所有概念及示例。此教程适合希望获得专业知识和深入理解 Golang 的人——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合你——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
