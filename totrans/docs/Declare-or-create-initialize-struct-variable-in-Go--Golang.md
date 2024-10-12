<!--yml
category: 未分类
date: 2024-10-13 06:19:50
-->

# Declare or create/initialize struct variable in Go (Golang

> 来源：[https://golangbyexample.com/declare-initialize-struct-variable-golang/](https://golangbyexample.com/declare-initialize-struct-variable-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Declaring a struct type](#Declaring_a_struct_type "Declaring a struct type")
*   [Creating a struct variable](#Creating_a_struct_variable "Creating a struct variable")*  *# **Overview**

GO struct is named collection of data fields which can be of different types. Struct acts as a container that has different heterogeneous data types which together represents an entity.

For example, different attributes are used to represent an employee in an organization. Employee can have

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

A struct in golang can be compared to a class in Object Oriented Languages

# **Declaring a struct type**

Below is the format for declaring a struct

```
type struct_name struct {
    field_name1 field_type1
    field_name2 field_type2
    ...
}
```

In the above format, **struct_name** is the name of the struct. It has a field named **field_name1** of type **field_type1** and a field named **field_name2** of type **field_type2**. This declares a new named struct type which acts as a blueprint. The type keyword is used to introduce a new type

Example

```
type point struct {
    x float64
    y float64
}
```

The above declaration declares a new struct named **point** which has two field **x** and **y**. Both fields are of **float64** type.Once a new struct type is declared we can define new concrete struct variable from it as we will see in next section

# **Creating a struct variable**

Declaring a struct just declares a named struct type. Creating a struct variable, creates an instance of that struct with memory being initialized as well. We can create a empty struct variable without given any value to any of the field

```
emp := employee{}
```

In this case, all the fields in the struct are initialized with a default zero value of that field type. We can also intialize value for each struct field while creating a struct variable. There are two variations

*   On the same line

```
emp := employee{name: "Sam", age: 31, salary: 2000}
```

*   Each field on different lines

```
emp := employee{
   name:   "Sam",
   age:    31,
   salary: 2000,
}
```

It is also ok to initialize only some of the fields with value. The field which are not initialized with value will get the default zero value of their type

```
emp := employee{
   name: "Sam",
   age: 31,
}
```

In the above case, salary will get the default value of zero since it is not initialized

Let’s see a working code illustrating above points:

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func main() {
    emp1 := employee{}
    fmt.Printf("Emp1: %+v\n", emp1)

    emp2 := employee{name: "Sam", age: 31, salary: 2000}
    fmt.Printf("Emp2: %+v\n", emp2)

    emp3 := employee{
        name:   "Sam",
        age:    31,
        salary: 2000,
    }
    fmt.Printf("Emp3: %+v\n", emp3)

    emp4 := employee{
        name: "Sam",
        age:  31,
    }
    fmt.Printf("Emp4: %+v\n", emp4)
}
```

**Output**

```
Emp1: {name: age:0 salary:0}
Emp2: {name:Sam age:31 salary:2000}
Emp2: {name:Sam age:31 salary:0}
```

For above program

*   We first declare an **employee** struct.

*   emp1’s fields are all intialized with default zero value of its type i.e name with “”, age and salary with 0.

*   emp2 has been initialized with all field on the same line. Its fields are correctly printed with their value
*   emp3’s has been initialized with all fields on different lines. Its fields are correctly printed with their value

*   emp4’s salary field is initialized with a default zero value of 0\. While other two fields are correctly printed with their value.

It is to be noted that in the initialization of a struct, every new line with in curly braces has to end with a comma. So below initialization will raise error as

```
 "salary" : 2000
```

doesn’t end with a comma.

```
emp := employee{
  name:   "Sam",
  age:    31,
  salary: 2000
}
```

The below will be fine as last brace is on the same line as last field

```
emp := employee{
  name:   "Sam",
  age:    31,
  salary: 2000}
```

**Without field names**

struct can also be initialized without specifying the field names. But in this case, all values for each of the field has to be provided in sequence

```
emp := employee{"Sam", 31, 2000}
```

A compiler error will be raised if all values are not provided when field name is not used.

Let’s see a program

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func main() {
    emp := employee{"Sam", 31, 2000}
    fmt.Printf("Emp: %+v\n", emp)

    //emp = employee{"Sam", 31}
}
```

**Output**

```
Emp2: {name:Sam age:31 salary:2000}
```

Uncomment the line

```
emp = employee{"Sam", 31}
```

in the above program, and it will raise compiler error

```
too few values in employee literal
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*