<!--yml
category: 未分类
date: 2024-10-13 06:19:39
-->

# Check if two structs are equal or struct equality in Go (Golang)

> 来源：[https://golangbyexample.com/struct-equality-golang/](https://golangbyexample.com/struct-equality-golang/)

The first thing to know before considering struct equality is weather if all struct fields types are comparable or not

Some of the comparable types as defined by go specification are

*   boolean
*   numeric
*   string,
*   pointer
*   channel
*   interface types
*   structs – if all it’s field type is comparable
*   array – if the type of value of array element is comparable

Some of the types which are not comparable as per go specification and which cannot be used as a key in a map are.

*   Slice
*   Map
*   Function

So two struct will be equal if first all their field types are comparable and all the corresponding field values are equal.

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
    emp1 := employee{name: "Sam", age: 31, salary: 2000}
    emp2 := employee{name: "Sam", age: 31, salary: 2000}
    if emp1 == emp2 {
        fmt.Println("emp1 annd emp2 are equal")
    } else {
        fmt.Println("emp1 annd emp2 are not equal")
    }
}
```

**Output**

```
emp1 annd emp2 are equal
```

If the struct field type are not comparable then there will be compilation error on checking struct equality using the == operator.

```
package main

import "fmt"

type employee struct {
    name        string
    age         int
    salary      int
    departments []string
}

func main() {
    emp1 := employee{name: "Sam", age: 31, salary: 2000, departments: []string{"CS"}}
    emp2 := employee{name: "Sam", age: 31, salary: 2000, departments: []string{"EC"}}
    if emp1 == emp2 {
        fmt.Println("emp1 annd emp2 are equal")
    } else {
        fmt.Println("emp1 annd emp2 are not equal")
    }
}
```

Above program will raise compilation error as **employee** struct contains a field **deparments** which is a **slice** of **string**. **slice** is not a comparable type and hence the compilation error.

```
invalid operation: emp1 == emp2 (struct containing []string cannot be compared)
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)