<!--yml
category: 未分类
date: 2024-10-13 06:11:06
-->

# Pass Variable Number of Arguments to a function in Go (Golang)

> 来源：[https://golangbyexample.com/go-variadic-function/](https://golangbyexample.com/go-variadic-function/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Different number of parameters but of the same type](#Different_number_of_parameters_but_of_the_same_type "Different number of parameters but of the same type")
*   [Different number of parameters and of different types](#Different_number_of_parameters_and_of_different_types "Different number of parameters and of different types")*  *# Overview

In Go, a function that can accept a dynamic number of arguments is called a Variadic function. Below is the syntax for variadic function. Three dots are used as a prefix before type.

```
func add(numbers ...int)
```

The above function can be called with zero or more arguments

```
add()
add(1,2)
add(1,2,3,4)
```

The variadic param which is **numbers,** in this case, gets converted into a slice inside the function which can be iterated using **range**

```
func add(numbers ...int) int {
    sum := 0
    for _, num := range numbers {
        sum += num
    }
    return sum
}
```

If you already have a slice and you need to pass it as a variadic param then that can be done by adding three dots (…) after the argument while calling the function

```
var numbers := []int{2,3,5}
add(numbers...)
```

In case there is a need to pass variadic as well as non-variadic arguments to a function, then the non-variadic needs to be passed as initial arguments and variadic argument need to be the last.

```
func add(val string, numbers ...int)
```

The best example of a variadic function used in GO library is the **fmt.Println()** function. This is the signature of the function

```
func Println(a ...interface{}) (n int, err error) 
```

While using variadic functions there can be a couple of cases related to the variadic argument.

# **Different number of parameters but of the same type**

The above case can easily be handled using variadic functions. Notice in below code the parameter is of one type i.e. **int.**

```
package main

import "fmt"

func main() {
    fmt.Println(add(1, 2))
    fmt.Println(add(1, 2, 3))
    fmt.Println(add(1, 2, 3, 4))
}

func add(numbers ...int) int {
    sum := 0
    for _, num := range numbers {
        sum += num
    }
    return sum
}
```

**Output:**

```
3
6
10
```

# **Different number of parameters and of different types**

This case can be handled using both variadic function and empty interface

```
package main

import "fmt"

func main() {
    handle(1, "abc")
    handle("abc", "xyz", 3)
    handle(1, 2, 3, 4)
}

func handle(params ...interface{}) {
    fmt.Println("Handle func called with parameters:")
    for _, param := range params {
        fmt.Printf("%v\n", param)
    }
}
```

**Output:**

```
Handle func called with parameters:
1
abc
Handle func called with parameters:
abc
xyz
3
Handle func called with parameters:
1
2
3
4
```

We can also use a switch case to get the exact parameters and use them accordingly. See the below example.

```
package main

import "fmt"

type person struct {
    name   string
    gender string
    age    int
}

func main() {
    err := addPerson("Tina", "Female", 20)
    if err != nil {
        fmt.Println("PersonAdd Error: " + err.Error())
    }

    err = addPerson("John", "Male")
    if err != nil {
        fmt.Println("PersonAdd Error: " + err.Error())
    }

    err = addPerson("Wick", 2, 3)
    if err != nil {
        fmt.Println("PersonAdd Error: " + err.Error())
    }
}

func addPerson(args ...interface{}) error {
    if len(args) > 3 {
        return fmt.Errorf("Wront number of arguments passed")
    }
    p := &person{}
    //0 is name
    //1 is gender
    //2 is age
    for i, arg := range args {
        switch i {
        case 0: // name
            name, ok := arg.(string)
            if !ok {
                return fmt.Errorf("Name is not passed as string")
            }
            p.name = name
        case 1:
            gender, ok := arg.(string)
            if !ok {
                return fmt.Errorf("Gender is not passed as string")
            }
            p.gender = gender
        case 2:
            age, ok := arg.(int)
            if !ok {
                return fmt.Errorf("Age is not passed as int")
            }
            p.age = age
        default:
            return fmt.Errorf("Wrong parametes passed")
        }
    }
    fmt.Printf("Person struct is %+v\n", p)
    return nil
}
```

**Note:** Wherever the arg is not passed it is substituted as default.

**Output:**

```
Person struct is &{name:Tina gender:Female age:20}
Person struct is &{name:John gender:Male age:0}
PersonAdd Error: Gender is not passed as string
```*