<!--yml

分类：未分类

日期：2024-10-13 06:19:59

-->

# Go (Golang) 中的嵌套结构体

> 来源：[`golangbyexample.com/nested-struct-golang/`](https://golangbyexample.com/nested-struct-golang/)

一个结构体可以嵌套另一个结构体。让我们来看一个嵌套结构体的例子。在下面的**employee**结构体中嵌套了**address**结构体。

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

注意如何访问嵌套结构体字段。

```go
emp.address.city
emp.address.country
```


