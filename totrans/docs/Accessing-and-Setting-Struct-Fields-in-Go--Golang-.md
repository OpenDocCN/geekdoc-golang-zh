<!--yml
category: 未分类
date: 2024-10-13 06:20:05
-->

# Accessing and Setting Struct Fields in Go (Golang)

> 来源：[https://golangbyexample.com/accessing-setting-struct-fields-golang/](https://golangbyexample.com/accessing-setting-struct-fields-golang/)

**Overview**

GO struct is named collection of data fields which can be of different types. Struct acts as a container that has different heterogeneous data types which together represents an entity. For example, different attributes are used to represent an employee in an organization. Employee can have

*   Name of string type

*   Age of int type

*   DOB of time.Time type

*   Salary of int type

.. and so on. A struct can be used to represent an employee

```
type employee struct {
    name   string
    age    int
    salary int
}
```

Table of Contents

 **   [Accessing and Setting Struct Fields](#Accessing_and_Setting_Struct_Fields "Accessing and Setting Struct Fields")*  *# **Accessing and Setting Struct Fields**

A struct variable can be created as below

```
emp := employee{name: "Sam", age: 31, salary: 2000}
```

Once the struct variable is created, structs fields can be accessed using the dot operator. Below is the format for getting the value

```
n := emp.name
```

Similarly a value can be assigned to a struct field too.

```
emp.name = "some_new_name"
```

Let’s see an example

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func main() {
    emp := employee{name: "Sam", age: 31, salary: 2000}

    //Accessing a struct field
    n := emp.name
    fmt.Printf("Current name is: %s\n", n)

    //Assigning a new value
    emp.name = "John"
    fmt.Printf("New name is: %s\n", emp.name)
}
```

**Output**

```
Current name is: Sam
New name is: John
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*