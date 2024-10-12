<!--yml
category: 未分类
date: 2024-10-13 06:19:59
-->

# Nested Struct in Go (Golang)

> 来源：[https://golangbyexample.com/nested-struct-golang/](https://golangbyexample.com/nested-struct-golang/)

A struct can have an another struct nested in it. Let’s see an example of a nested struct. In below **employee** struct has **address** struct nested it in.

```
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

**Output**

```
City: London
Country: UK
```

Notice how nested struct fields are accessed.

```
emp.address.city
emp.address.country
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)