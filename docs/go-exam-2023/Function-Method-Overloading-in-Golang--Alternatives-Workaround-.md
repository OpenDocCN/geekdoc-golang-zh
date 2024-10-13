<!--yml

分类：未分类

日期：2024-10-13 06:04:57

-->

# Golang中的函数/方法重载（替代方案/变通方法）

> 来源：[https://golangbyexample.com/function-method-overloading-golang/](https://golangbyexample.com/function-method-overloading-golang/)

函数/方法重载意味着可以使用相同的函数/方法名称，并具有不同数量和类型的参数

请参见此帖子了解Go中函数和方法之间的区别 – [https://golangbyexample.com/difference-between-method-function-go](https://golangbyexample.com/difference-between-method-function-go)

例如。

```go
func X()
func X(name string)
func X(name, address string)
func X(name string, age int)
```

Go不支持方法/函数重载。请查看此常见问题以了解原因 [https://golang.org/doc/faq#overloading](https://golang.org/doc/faq#overloading)

根据上述常见问题，没有它一切都更简单。

我们可以通过使用变参函数来变通GO中的方法/函数重载

+   **变参函数** – 变参函数是接受可变数量参数的函数

+   **空接口** – 这是一个没有任何方法的接口。

方法/函数重载有两种情况

1.**不同数量的参数，但类型相同：**

上述情况可以通过变参函数轻松处理。请注意，以下代码中的参数都是同一类型，即**int。**

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
```

**输出：**

```go
3
6
10
```

2.**不同数量的参数和不同类型**

该情况可以使用变参函数和空接口来处理

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

我们还可以使用switch语句来获取确切的参数并相应使用。请参见以下示例。

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

**注意：** 无论何时未传递参数，都会替代为默认值。

**输出：**

```go
Person struct is &{name:Tina gender:Female age:20}
Person struct is &{name:John gender:Male age:0}
PersonAdd Error: Gender is not passed as string
```

+   [函数](https://golangbyexample.com/tag/function/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [方法](https://golangbyexample.com/tag/method/)*   [重载](https://golangbyexample.com/tag/overloading/)
