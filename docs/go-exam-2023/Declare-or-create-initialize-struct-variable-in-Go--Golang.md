<!--yml

分类：未分类

日期：2024-10-13 06:19:50

-->

# 在 Go（Golang）中声明或创建/初始化结构变量

> 来源：[https://golangbyexample.com/declare-initialize-struct-variable-golang/](https://golangbyexample.com/declare-initialize-struct-variable-golang/)

目录

**   [概述](#Overview "概述")

+   [声明一个结构类型](#Declaring_a_struct_type "声明一个结构类型")

+   [创建一个结构变量](#Creating_a_struct_variable "创建一个结构变量")*  *# **概述**

GO 结构是不同类型数据字段的命名集合。结构充当容器，具有不同的异构数据类型，共同表示一个实体。

例如，不同的属性用于表示组织中的员工。员工可以拥有

+   字符串类型的名称

+   整数类型的年龄

+   时间类型的出生日期

+   整数类型的薪水

.. 以及其他内容。结构可以用于表示员工

```
type employee struct {
    name   string
    age    int
    salary int
}
```

在 Golang 中，结构可以与面向对象语言中的类相比较

# **声明一个结构类型**

以下是声明结构的格式

```
type struct_name struct {
    field_name1 field_type1
    field_name2 field_type2
    ...
}
```

在上述格式中，**struct_name** 是结构的名称。它有一个名为 **field_name1** 的字段，类型为 **field_type1**，还有一个名为 **field_name2** 的字段，类型为 **field_type2**。这声明了一个新的命名结构类型，作为蓝图。type 关键字用于引入新类型

示例

```
type point struct {
    x float64
    y float64
}
```

上述声明声明了一个名为 **point** 的新结构，它有两个字段 **x** 和 **y**。两个字段都是 **float64** 类型。一旦声明了新的结构类型，我们可以从中定义新的具体结构变量，如下节所示

# **创建一个结构变量**

声明一个结构只是声明一个命名的结构类型。创建一个结构变量，会创建该结构的一个实例，同时内存也会被初始化。我们可以创建一个空的结构变量，而不为任何字段赋值

```
emp := employee{}
```

在这种情况下，结构中的所有字段都被初始化为该字段类型的默认零值。我们在创建结构变量时也可以为每个结构字段初始化值。这里有两种变体

+   在同一行

```
emp := employee{name: "Sam", age: 31, salary: 2000}
```

+   每个字段在不同的行上

```
emp := employee{
   name:   "Sam",
   age:    31,
   salary: 2000,
}
```

仅初始化某些字段的值也是可以的。未初始化的字段将获得其类型的默认零值

```
emp := employee{
   name: "Sam",
   age: 31,
}
```

在上述情况下，薪水将得到默认的零值，因为它没有被初始化

让我们看看一个演示上述要点的工作代码：

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

**输出**

```
Emp1: {name: age:0 salary:0}
Emp2: {name:Sam age:31 salary:2000}
Emp2: {name:Sam age:31 salary:0}
```

对于上述程序

+   我们首先声明一个 **employee** 结构。

+   emp1 的所有字段都被初始化为其类型的默认零值，即名称为“”，年龄和薪水为 0。

+   emp2 已在同一行上初始化了所有字段。其字段正确打印了它们的值

+   emp3 的所有字段都在不同的行上初始化。其字段正确打印了它们的值

+   emp4 的薪水字段被初始化为默认零值 0。其他两个字段正确打印了它们的值。

需要注意的是，在结构体的初始化中，每个在大括号内的新行必须以逗号结尾。因此，下面的初始化会引发错误，因为

```
 "salary" : 2000
```

不以逗号结束。

```
emp := employee{
  name:   "Sam",
  age:    31,
  salary: 2000
}
```

下面的代码是可以的，因为最后的括号与最后一个字段在同一行。

```
emp := employee{
  name:   "Sam",
  age:    31,
  salary: 2000}
```

**没有字段名称**

结构体也可以在不指定字段名称的情况下进行初始化。但在这种情况下，每个字段的所有值必须按顺序提供。

```
emp := employee{"Sam", 31, 2000}
```

如果没有使用字段名称而没有提供所有值，将会引发编译器错误。

让我们来看一个程序。

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

**输出**

```
Emp2: {name:Sam age:31 salary:2000}
```

取消注释这一行。

```
emp = employee{"Sam", 31}
```

在上述程序中，它将引发编译器错误。

```
too few values in employee literal
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
