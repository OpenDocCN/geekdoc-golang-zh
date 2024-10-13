<!--yml

category: 未分类

date: 2024-10-13 06:06:10

-->

# 在 Go 中排序自定义结构（Golang）

> 来源：[`golangbyexample.com/sort-custom-struct-collection-golang/`](https://golangbyexample.com/sort-custom-struct-collection-golang/)

目录

**   介绍

+   完整工作代码：*  *# **介绍**

GO 有一个 **sort** 包，提供了用于排序切片和用户定义类型的实用工具原语。任何集合只要实现了 **sort.Interface**，就可以通过 GO 的 **sort** 包的 **Sort** 函数进行排序。

以下是 sort.Interface 的方法。

[`golang.org/pkg/sort/#Interface`](https://golang.org/pkg/sort/#Interface)

```go
type Interface interface {
    // Len is the number of elements in the collection.
    Len() int

    // Less reports whether the element with
    // index i should sort before the element with index j.
    Less(i, j int) bool

    // Swap swaps the elements with indexes i and j.
    Swap(i, j int)
}
```

让我们看一个工作示例，说明如何使用 **sort.Interface** 对用户定义的结构体进行排序。在下面的示例中。

+   我们创建了一个名为 **employee** 的自定义结构，字段包括 **name** 和以美元为单位的 **salary**。

+   我们有一个 **employeeList**，它保存 **employee** 的列表。

+   **employeeList** 实现了 **Len()、Less()、Swap()** 方法，因此它实现了 **sort.Interface**。

+   我们按薪资从高到低对员工进行排序。要排序 **employeeList**，我们将其传递给 **sort.Sort()** 函数。

# **完整工作代码：**

```go
package main

import (
    "fmt"
    "sort"
)

type employee struct {
    name   string
    salary int
}

type employeeList []employee

func (e employeeList) Len() int {
    return len(e)
}

func (e employeeList) Less(i, j int) bool {
    return e[i].salary > e[j].salary
}

func (e employeeList) Swap(i, j int) {
    e[i], e[j] = e[j], e[i]
}

func main() {
    eList := []employee{
        employee{name: "John", salary: 3000},
        employee{name: "Bill", salary: 4000},
        employee{name: "Sam", salary: 1000},
    }
    sort.Sort(employeeList(eList))
    for _, employee := range eList {
        fmt.Printf("Name: %s Salary %d\n", employee.name, employee.salary)
    }
}
```

**输出：**

```go
Name: Bill Salary 4000
Name: John Salary 3000
Name: Sam Salary 1000
```

要从最低薪资排序到最高薪资，我们需要用 **‘>’** 符号更改 **Less** 函数。

```go
func (e employeeList) Less(i, j int) bool {
    return e[i].salary > e[j].salary
}
```

修改后，当我们运行程序时，输出将是：

```go
Name: Sam Salary 1000
Name: John Salary 3000
Name: Bill Salary 4000
```

+   [array](https://golangbyexample.com/tag/array/)*   [custom](https://golangbyexample.com/tag/custom/)*   [go](https://golangbyexample.com/tag/go/)*   [structs](https://golangbyexample.com/tag/structs/)*
