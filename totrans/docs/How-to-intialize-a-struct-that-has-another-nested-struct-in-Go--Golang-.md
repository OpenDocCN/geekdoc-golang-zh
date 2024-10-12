<!--yml
category: 未分类
date: 2024-10-13 06:34:16
-->

# How to intialize a struct that has another nested struct in Go (Golang)

> 来源：[https://golangbyexample.com/struct-init-nested-struct-go/](https://golangbyexample.com/struct-init-nested-struct-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

A struct can have another struct nested in it. Let’s see an example of a nested struct. In below example **employee** struct has the **address** struct nested it in.

```
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

To initialize such kind of struct we need to initialize the nested struct first. That is first we will initialize the **address** struct. For eg as below

```
address := address{city: "London", country: "UK"}
```

Then we can initialize the **employee** struct as below

```
emp := employee{name: "Sam", age: 31, salary: 2000, address: address}
```

Another way is to directly initialize the address struct during the initialization of the employee struct. Like as below

```
emp := employee{name: "Sam", age: 31, salary: 2000, address: address{city: "London", country: "UK"}}
```

# **Program**

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*