<!--yml

类别：未分类

日期：2024-10-13 06:19:39

-->

# 检查两个结构体是否相等或在 Go (Golang) 中的结构体相等性。

> 来源：[`golangbyexample.com/struct-equality-golang/`](https://golangbyexample.com/struct-equality-golang/)

在考虑结构体相等性之前，首先要知道所有结构体字段的类型是否可比较。

根据 Go 规范定义的一些可比较类型包括：

+   布尔值

+   数值

+   字符串，

+   指针

+   渠道

+   接口类型

+   结构体 – 如果它的所有字段类型都是可比较的。

+   数组 – 如果数组元素的值类型是可比较的。

根据 Go 规范，不可比较的类型以及不能用作映射键的类型包括：

+   切片

+   映射

+   函数

如果两个结构体的所有字段类型可比较且所有对应字段值相等，则它们是相等的。

让我们看一个例子。

```go
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

**输出**

```go
emp1 annd emp2 are equal
```

如果结构体字段类型不可比较，则在使用 == 运算符检查结构体相等性时会出现编译错误。

```go
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

上述程序会引发编译错误，因为 **employee** 结构体包含一个字段 **departments**，这是一个 **slice** 类型的 **string**。**slice** 不是可比较类型，因此会出现编译错误。

```go
invalid operation: emp1 == emp2 (struct containing []string cannot be compared)
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
