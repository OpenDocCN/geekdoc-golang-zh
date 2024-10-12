<!--yml
category: 未分类
date: 2024-10-13 06:06:10
-->

# Sort a Custom Struct in Go (Golang)

> 来源：[https://golangbyexample.com/sort-custom-struct-collection-golang/](https://golangbyexample.com/sort-custom-struct-collection-golang/)

Table of Contents

 **   [Introduction](#Introduction "Introduction")
*   [Full Working Code:](#Full_Working_Code "Full Working Code:")*  *# **Introduction**

GO has a **sort** package that provides utility primitives for the sorting of slices and user-defined types. Any collection can be sorted by the **Sort** function of **sort** package of GO it if implements the **sort.Interface**.

Below are the methods of sort.Interface

[https://golang.org/pkg/sort/#Interface](https://golang.org/pkg/sort/#Interface)

```
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

Let’s see a working example to illustrate how we can use **sort.Interface** to sort a user-defined struct. In below example

*   We created a custom struct called **employee** with **name** and **salary** in dollars as fields

*   We have a **employeeList** which hold the list of **employee**.

*   **employeeList** implements the **Len(), Less(), Swap()** method hence it implements the **sort.Interface**

*   We sort the employee from highest salary to lowest salary. To sort employeeList we pass it to **sort.Sort()** function

# **Full Working Code:**

```
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

**Output:**

```
Name: Bill Salary 4000
Name: John Salary 3000
Name: Sam Salary 1000
```

To sort from lowest salary to highest salary we need to change the **Less** function with **‘>’** sign

```
func (e employeeList) Less(i, j int) bool {
    return e[i].salary > e[j].salary
}
```

After changing it when we run the program then output will be:

```
Name: Sam Salary 1000
Name: John Salary 3000
Name: Bill Salary 4000
```

*   [array](https://golangbyexample.com/tag/array/)*   [custom](https://golangbyexample.com/tag/custom/)*   [go](https://golangbyexample.com/tag/go/)*   [structs](https://golangbyexample.com/tag/structs/)*