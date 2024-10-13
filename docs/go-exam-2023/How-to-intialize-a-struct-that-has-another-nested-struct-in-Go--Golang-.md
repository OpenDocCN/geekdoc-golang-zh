<!--yml

分类：未分类

日期：2024-10-13 06:34:16

-->

# 如何在 Go（Golang）中初始化一个包含嵌套结构体的结构体

> 来源：[`golangbyexample.com/struct-init-nested-struct-go/`](https://golangbyexample.com/struct-init-nested-struct-go/)

目录

+   概述

+   程序

# **概述**

一个结构体可以嵌套另一个结构体。让我们来看一个嵌套结构体的示例。在下面的示例中，**员工**结构体嵌套了**地址**结构体。

```go
type address struct {
    city    string
    country string
}

type employee struct {
    name    string
    age     int
    salary  int
    address address
}
```

要初始化这种结构体，我们需要先初始化嵌套结构体。也就是说，我们首先要初始化**地址**结构体。例如如下所示

```go
address := address{city: "London", country: "UK"}
```

然后我们可以如下初始化**员工**结构体

```go
emp := employee{name: "Sam", age: 31, salary: 2000, address: address}
```

另一种方法是在员工结构体初始化时直接初始化地址结构体。如下所示

```go
emp := employee{name: "Sam", age: 31, salary: 2000, address: address{city: "London", country: "UK"}}
```

# **程序**

```go
package main

import "fmt"

type employee struct {
    name    string
    age     int
    salary  int
    address address
}

type address struct {
    city    string
    country string
}

func main() {
    address := address{city: "London", country: "UK"}
    emp := employee{name: "Sam", age: 31, salary: 2000, address: address}
    fmt.Printf("City: %s\n", emp.address.city)
    fmt.Printf("Country: %s\n", emp.address.country)
}
```

**输出**

```go
City: London
Country: UK
```


