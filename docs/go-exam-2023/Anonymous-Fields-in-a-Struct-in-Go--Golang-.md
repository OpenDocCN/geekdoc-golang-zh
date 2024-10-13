<!--yml

分类：未分类

日期：2024-10-13 06:19:55

-->

# Go (Golang) 中的结构体中的匿名字段。

> 来源：[`golangbyexample.com/anonymous-fields-struct-golang/`](https://golangbyexample.com/anonymous-fields-struct-golang/)

结构体也可以包含匿名字段，意味着一个字段没有名称。类型将成为字段名。在下面的示例中，**string** 也将作为字段名。

```go
type employee struct {
    string
    age    int
    salary int
}
```

匿名字段也可以被访问并赋值。

```go
package main

import "fmt"

type employee struct {
    string
    age    int
    salary int
}

func main() {
    emp := employee{string: "Sam", age: 31, salary: 2000}
    //Accessing a struct field
    n := emp.string
    fmt.Printf("Current name is: %s\n", n)
    //Assigning a new value
    emp.string = "John"
    fmt.Printf("New name is: %s\n", emp.string)
}
```

**输出**

```go
Current name is: Sam
New name is: John
```

**嵌套结构体**

结构体可以嵌套另一个结构体。让我们看看一个嵌套结构体的示例。在下面的 **employee** 结构体中，嵌套了 **address** 结构体。

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

注意嵌套结构体字段是如何被访问的。

```go
emp.address.city
emp.address.country
```

目录

**匿名嵌套结构体字段

# **匿名嵌套结构体字段**

嵌套结构体字段也可以是匿名的。此外，在这种情况下，嵌套结构体的字段可以直接访问。因此，下面是有效的。

```go
emp.city
emp.country
```

还需注意，下面的代码在这种情况下仍然有效。

```go
emp.address.city
emp.address.country
```

让我们看一个程序。

```go
package main

import "fmt"

type employee struct {
	name   string
	age    int
	salary int
	address
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

	fmt.Printf("City: %s\n", emp.city)
	fmt.Printf("Country: %s\n", emp.country)
}
```

**输出**

```go
City: London
Country: UK
City: London
Country: UK
```

注意在上述程序中，地址结构体的城市字段可以通过两种方式访问。

```go
emp.city
emp.address.city
```

地址结构体的国家字段也是类似的。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
