<!--yml

分类：未分类

日期：2024-10-13 06:19:24

-->

# Go（Golang）中的结构体指针

> 来源：[`golangbyexample.com/pointer-to-struct-golang/`](https://golangbyexample.com/pointer-to-struct-golang/)

在 Golang 中，结构体是命名的数据字段集合。这些字段可以是不同类型。结构体充当容器，包含不同的异构数据类型，共同表示一个实体。例如，不同的属性用于表示组织中的员工。员工可以有：

+   字符串类型的名称

+   年龄类型为 int

+   DOB 类型为 time.Time

+   薪水类型为 int

Golang 中的指针是一个变量，它保存另一个变量的内存地址。

现在我们已经理解了结构体和指针，接下来让我们看看如何定义指向结构体的指针。

假设我们有一个员工结构体

```go
type employee struct {
    name   string
    age    int
    salary int
}
```

创建指向结构体的指针有两种方法

+   使用**&**运算符

+   使用 new 关键字

让我们逐一查看上述方法。

**使用 & 运算符**

**&** 运算符可用于获取结构体变量的指针。

```go
emp := employee{name: "Sam", age: 31, salary: 2000}
empP := &emp
```

结构体指针也可以直接创建

```go
empP := &employee{name: "Sam", age: 31, salary: 2000}
```

让我们看一个程序

```go
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

**输出**

```go
Emp: &{name:Sam age:31 salary:2000}
Emp: &{name:John age:30 salary:3000}
```

**使用 new 关键字**

使用 new() 关键字将：

+   创建结构体

+   将所有字段初始化为其类型的零默认值

+   返回指向新创建结构体的指针

这将返回一个指针

```go
empP := new(employee)
```

指针地址可以使用**%p**格式修饰符打印

```go
fmt.Printf("Emp Pointer: %p\n", empP)
```

还要注意，*运算符可用于解引用指针，这意味着获取存储在指针中的地址的值。

```go
fmt.Printf("Emp Value: %+v\n", *empP)
```

它将打印

```go
Emp Value: {name: age:0 salary:0}
```

当不使用解引用指针而使用格式标识符**%+v**时，将在结构体之前附加&符号，表示这是一个指针。

```go
fmt.Printf("Emp Value: %+v\n", empP)
```

它将打印

```go
Emp Value: &{name: age:0 salary:0}
```

让我们看看完整程序以说明上述要点

```go
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

**输出**

```go
Emp Pointer: 0xc0000a6020
Emp Value: &{name: age:0 salary:0}
Emp Value: {name: age:0 salary:0}
```

打印的指针地址在你的机器上会有所不同。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
