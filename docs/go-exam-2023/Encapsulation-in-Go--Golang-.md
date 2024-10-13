<!--yml

类别：未分类

日期：2024-10-13 06:03:22

-->

# Go（Golang）中的封装

> 来源：[https://golangbyexample.com/encapsulation-in-go/](https://golangbyexample.com/encapsulation-in-go/)

Golang 在包级别提供封装。Go 没有公共、私有或受保护的关键字。控制可见性的唯一机制是使用大写和小写格式

+   **大写标识符** 是导出的。大写字母表示这是一个导出标识符。

+   **小写标识符** 是非导出的。小写字母表示该标识符是非导出的，只能在同一包内访问。

有五种标识符可以是导出或非导出的

1.  结构

1.  结构的方法

1.  结构体字段

1.  函数

1.  变量

让我们看一个例子，展示上述所有标识符的导出和非导出。见下方的 **data.go**。包是 **model**

+   结构

    +   结构体 **Person** 是导出的

    +   结构体 **company** 是非导出的

+   结构的方法

    +   **Person** 结构体的方法 **GetAge()** 是导出的

    +   **Person** 结构体的方法 **getName()** 是非导出的

+   结构体字段

    +   **Person** 结构体字段 **Name** 是导出的

    +   **Person** 结构体字段 **age** 是非导出的

+   函数

    +   函数 **GetPerson()** 是导出的

    +   函数 **getCompanyName()** 是非导出的

+   变量

    +   变量 **CompanyName** 是导出的

    +   变量 **companyLocation** 是非导出的

**data.go**

```go
package model

import "fmt"

var (
    //CompanyName represents the company name
    CompanyName     = "test"
    companyLocation = "somecity"
)

//Person struct
type Person struct {
    Name string
    age  int
}

//GetAge of person
func (p *Person) GetAge() int {
    return p.age
}

func (p *Person) getName() string {
    return p.Name
}

type company struct {
}

//GetPerson get the person object
func GetPerson() *Person {
    p := &Person{
        Name: "test",
        age:  21,
    }
    fmt.Println("Model Package:")
    fmt.Println(p.Name)
    fmt.Println(p.age)
    return p
}

func getCompanyName() string {
    return CompanyName
}
```

我们在 **model** 包中写一个文件 **test.go**。见下文。

**test.go**

```go
package model

import "fmt"

//Test function
func Test() {
    //STRUCTURE IDENTIFIER
    p := &Person{
        Name: "test",
        age:  21,
    }
    fmt.Println(p)
    c := &company{}
    fmt.Println(c)

    //STRUCTURE'S METHOD
    fmt.Println(p.GetAge())
    fmt.Println(p.getName())

    //STRUCTURE'S FIELDS
    fmt.Println(p.Name)
    fmt.Println(p.age)

    //FUNCTION
    person2 := GetPerson()
    fmt.Println(person2)
    companyName := getCompanyName()
    fmt.Println(companyName)

    //VARIBLES
    fmt.Println(companyLocation)
    fmt.Println(CompanyName)
}
```

运行这个文件时，它能够访问 **data.go** 中的所有导出和非导出字段，因为两者都位于同一个模型包中。没有编译错误，输出如下

**输出：**

```go
&{test 21}
&{}
21
test
test
21
Model Package:
test
21
&{test 21}
test
somecity
test
```

我们将上面的文件 **test.go** 移动到一个名为 **view** 的不同包中。现在注意运行‘go build’时的输出。它会产生编译错误。所有的编译错误都是因为无法引用非导出字段

**test.go**

```go
package view

import "fmt"

//Test function
func Test() {
    //STRUCTURE IDENTIFIER
    p := &model.Person{
        Name: "test",
        age:  21,
    }
    fmt.Println(p)
    c := &model.company{}
    fmt.Println(c)

    //STRUCTURE'S METHOD
    fmt.Println(p.GetAge())
    fmt.Println(p.getName())

    //STRUCTURE'S FIELDS
    fmt.Println(p.Name)
    fmt.Println(p.age)

    //FUNCTION
    person2 := model.GetPerson()
    fmt.Println(person2)
    companyName := model.getCompanyName()
    fmt.Println(companyName)

    //VARIBLES
    fmt.Println(model.companyLocation)
    fmt.Println(model.CompanyName)
}
```

**输出：**

```go
test.go:13:3: unknown field 'age' in struct literal of type model.Person
test.go:17:8: cannot refer to unexported name model.company
test.go:17:8: undefined: model.company
test.go:22:15: p.getName undefined (cannot refer to unexported field or method model.(*Person).getName)
test.go:26:15: p.age undefined (cannot refer to unexported field or method age)
test.go:31:17: cannot refer to unexported name model.getCompanyName
test.go:31:17: undefined: model.getCompanyName
test.go:35:14: cannot refer to unexported name model.companyLocation
test.go:35:14: undefined: model.companyLocation
```

+   [封装](https://golangbyexample.com/tag/encapsulation/) *   [golang中的封装](https://golangbyexample.com/tag/encapsulation-in-golang/) *   [面向对象的Go](https://golangbyexample.com/tag/object-oriented-go/) *   [面向对象编程](https://golangbyexample.com/tag/oop/) *   [Go中的面向对象编程](https://golangbyexample.com/tag/oop-in-go/)
