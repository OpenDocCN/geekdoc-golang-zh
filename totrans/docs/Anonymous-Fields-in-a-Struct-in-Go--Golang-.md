<!--yml
category: 未分类
date: 2024-10-13 06:19:55
-->

# Anonymous Fields in a Struct in Go (Golang)

> 来源：[https://golangbyexample.com/anonymous-fields-struct-golang/](https://golangbyexample.com/anonymous-fields-struct-golang/)

A struct can have anonymous fields as well, meaning a field having no name. The type will become the field name. In below example, **string** will be the field name as well

```
type employee struct {
    string
    age    int
    salary int
}
```

The anonymous field can also be accessed and assigned a value

```
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

**Output**

```
Current name is: Sam
New name is: John
```

**Nested Struct**

A struct can have another struct nested in it. Let’s see an example of a nested struct. In below **employee** struct has **address** struct nested it in.

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

Table of Contents

 **   [Anonymous nested struct fields](#Anonymous_nested_struct_fields "Anonymous nested struct fields")*  *# **Anonymous nested struct fields**

The nested struct field can also be anonymous. Also, in this case, nested struct’s fields are directly accessed. So below is valid

```
emp.city
emp.country
```

It is also to be noted that below is still valid in this case

```
emp.address.city
emp.address.country
```

Let’s see a program

```
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

**Output**

```
City: London
Country: UK
City: London
Country: UK
```

Notice in above program that city field of address struct can be accessed in two ways

```
emp.city
emp.address.city
```

Similar for the country field of the address struct.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*