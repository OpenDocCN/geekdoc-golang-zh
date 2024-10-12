<!--yml
category: 未分类
date: 2024-10-13 06:19:24
-->

# Pointer to a struct in Go (Golang)

> 来源：[https://golangbyexample.com/pointer-to-struct-golang/](https://golangbyexample.com/pointer-to-struct-golang/)

In golang struct is named collection of data fields. These fields can be of different types. Struct acts as a container that has different heterogeneous data types which together represents an entity. For example, different attributes are used to represent an employee in an organization. Employee can have:

*   Name of string type
*   Age of int type
*   DOB of time.Time type
*   Salary of int type

A pointer in golang is a variable that holds a memory address of another variable.

Now we have understood what struct and pointer is, let’s move unto how we can define a pointer to the struct.

Let’s say we have an employee struct

```
type employee struct {
    name   string
    age    int
    salary int
}
```

Thereare two ways of creating a pointer to the struct

*   Using the & operator

*   Using the new keyword

Let’s looks at each of above method one by one.

**Using the & operator**

The **&** operator can be used to get the pointer to a struct variable.

```
emp := employee{name: "Sam", age: 31, salary: 2000}
empP := &emp
```

struct pointer can also be directly created as well

```
empP := &employee{name: "Sam", age: 31, salary: 2000}
```

Let’s look at a program

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
    empP := &emp
    fmt.Printf("Emp: %+v\n", empP)
    empP = &employee{name: "John", age: 30, salary: 3000}
    fmt.Printf("Emp: %+v\n", empP)
}
```

**Output**

```
Emp: &{name:Sam age:31 salary:2000}
Emp: &{name:John age:30 salary:3000}
```

**Using the new keyword**

Using the  new() keyword will:

*   Create the struct

*   Initialize all the field to the zero default value of their type

*   Return the pointer to the newly created struct

This will return a pointer

```
empP := new(employee)
```

Pointer address can be print using the **%p** format modifier

```
fmt.Printf("Emp Pointer: %p\n", empP)
```

Also to note that * operator can be used to dereference a pointer which means getting the value at address stored in the pointer.

```
fmt.Printf("Emp Value: %+v\n", *empP)
```

It will print

```
Emp Value: {name: age:0 salary:0}
```

When not using the dereference pointer but using the format identifier**%+v,** then ampersand will be appended before the struct indicating that is a pointer.

```
fmt.Printf("Emp Value: %+v\n", empP)
```

It will print

```
Emp Value: &{name: age:0 salary:0}
```

Let’s see full program denoting above points

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func main() {
    empP := new(employee)
    fmt.Printf("Emp Pointer Address: %p\n", empP)
    fmt.Printf("Emp Pointer: %+v\n", empP)
    fmt.Printf("Emp Value: %+v\n", *empP)
}
```

**Output**

```
Emp Pointer: 0xc0000a6020
Emp Value: &{name: age:0 salary:0}
Emp Value: {name: age:0 salary:0}
```

The pointer address printed will be different on your machine.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)