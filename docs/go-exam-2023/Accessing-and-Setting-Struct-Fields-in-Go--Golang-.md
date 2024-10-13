<!--yml

分类: 未分类

日期: 2024-10-13 06:20:05

-->

# 在 Go (Golang) 中访问和设置结构体字段

> 来源：[https://golangbyexample.com/accessing-setting-struct-fields-golang/](https://golangbyexample.com/accessing-setting-struct-fields-golang/)

**概述**

Go 结构体是命名的数据字段集合，可以包含不同类型的数据。结构体作为一个容器，具有不同的异构数据类型，共同表示一个实体。例如，不同的属性用于表示一个组织中的员工。员工可以有

+   字符串类型的姓名

+   整型的年龄

+   time.Time 类型的出生日期

+   整型的工资

.. 以及其他。结构体可以用来表示一个员工

```
type employee struct {
    name   string
    age    int
    salary int
}
```

目录

**   [访问和设置结构体字段](#Accessing_and_Setting_Struct_Fields "Accessing and Setting Struct Fields")*  *# **访问和设置结构体字段**

结构体变量可以如下创建

```
emp := employee{name: "Sam", age: 31, salary: 2000}
```

一旦创建了结构体变量，可以使用点运算符访问结构体字段。以下是获取值的格式

```
n := emp.name
```

同样也可以给结构体字段赋值。

```
emp.name = "some_new_name"
```

让我们看一个例子

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

**输出**

```
Current name is: Sam
New name is: John
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
