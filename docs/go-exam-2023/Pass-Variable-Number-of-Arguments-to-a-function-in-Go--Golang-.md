<!--yml

类别：未分类

日期：2024-10-13 06:11:06

-->

# 在 Go (Golang) 中向函数传递可变数量的参数。

> 来源：[`golangbyexample.com/go-variadic-function/`](https://golangbyexample.com/go-variadic-function/)

目录

**   概述

+   不同数量的参数但类型相同

+   不同数量的参数且类型不同*  *# 概述

在 Go 中，能够接受动态数量参数的函数称为变长函数。下面是变长函数的语法。三个点用作类型前缀。

```go
func add(numbers ...int)
```

上述函数可以用零个或多个参数调用。

```go
add()
add(1,2)
add(1,2,3,4)
```

在此情况下，变长参数**numbers**会在函数内部转换为切片，可以使用**range**进行迭代。

```go
func add(numbers ...int) int {
    sum := 0
    for _, num := range numbers {
        sum += num
    }
    return sum
}
```

如果你已经有一个切片，并且需要将其作为变长参数传递，那么可以在调用函数时在参数后添加三个点（…）。

```go
var numbers := []int{2,3,5}
add(numbers...)
```

如果需要将变长参数和非变长参数传递给函数，则非变长参数需要作为初始参数传递，而变长参数需要作为最后一个参数传递。

```go
func add(val string, numbers ...int)
```

在 GO 库中，变长函数的最佳示例是**fmt.Println()**函数。以下是该函数的签名。

```go
func Println(a ...interface{}) (n int, err error) 
```

使用变长函数时，可能会出现与变长参数相关的一些情况。

# **不同数量的参数但类型相同**

上述案例可以很容易地通过变长函数处理。注意下面的代码中参数是单一类型，即**int。**

```go
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

**输出：**

```go
3
6
10
```

# **不同数量的参数且类型不同**

这个案例可以使用变长函数和空接口来处理。

```go
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

**输出：**

```go
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

我们还可以使用 switch case 来获取精确的参数并相应地使用它们。请参见下面的示例。

```go
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

**注意：** 如果没有传递参数，则用默认值替代。

**输出：**

```go
Person struct is &{name:Tina gender:Female age:20}
Person struct is &{name:John gender:Male age:0}
PersonAdd Error: Gender is not passed as string
```*
