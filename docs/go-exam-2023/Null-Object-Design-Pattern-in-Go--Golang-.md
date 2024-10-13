<!--yml

类别：未分类

日期：2024-10-13 06:03:37

-->

# Go 中的空对象设计模式（Golang）。

> 来源：[`golangbyexample.com/null-object-design-pattern-golang/`](https://golangbyexample.com/null-object-design-pattern-golang/)

注意：想了解其他所有设计模式如何在 GO 中实现，请查看这个完整参考 – [Go 中的所有设计模式](https://golangbyexample.com/all-design-patterns-golang/)

目录

**   简介：

+   完整工作代码：*  *## **简介：**

空对象设计模式是一种行为设计模式。当客户端代码依赖于某些可能为 null 的依赖项时，它非常有用。使用此设计模式可以防止客户端在这些依赖项的结果上进行 null 检查。值得注意的是，客户端的行为对于这些 null 依赖项也是可以接受的。

空对象设计模式的主要组件是：

1.  **实体** – 它是定义子结构体必须实现的原始操作的接口。

1.  **ConcreteEntity** – 它实现了实体接口。

1.  **NullEntity** – 它表示 null 对象。它也实现了实体接口，但具有 null 属性。

1.  **客户端** – 客户端获取实体接口的实现并使用它。它并不关心实现是 ConcreteEntity 还是 NullEntity。它将两者视为相同。

让我们考虑一个例子。假设我们有一个学院，里面有多个部门，每个部门有一些教授。

**department** 由一个接口表示。

```go
type department interface {
    getNumberOfProfessors() int
    getName() string
}
```

其中**college**表示为

```go
type college struct {
    departments []department
}
```

现在假设有一个机构想要计算特定学院特定部门的教授总数。我们将使用空对象设计模式处理这种情况，如果一个部门在学院中不存在，学院将返回一个 nullDepartment 对象（见**nullDepartment.go**）。

注意 agency.go 中的代码。

+   **agency.go** 根本不关心特定部门是否存在于学院中。如果该部门在学院中不存在，**college** 会返回一个 null 部门对象。

+   它将 nullDepartment 和真实部门视为相同，因此避免了 null 检查。它在两个对象上调用**getNumberOfProfessors()**。

以上是我们通过使用 null 对象设计模式在这种情况下获得的两个优点。请参见下面的代码。

**agency.go**

```go
package main

import "fmt"

func main() {
    college1 := createCollege1()
    college2 := createCollege2()
    totalProfessors := 0
    departmentArray := []string{"computerscience", "mechanical", "civil", "electronics"}

    for _, deparmentName := range departmentArray {
        d := college1.getDepartment(deparmentName)
        totalProfessors += d.getNumberOfProfessors()
    }

    fmt.Printf("Total number of professors in college1 is %d\n", totalProfessors)

    //Reset the professor count
    totalProfessors := 0
    for _, deparmentName := range departmentArray {
        d := college2.getDepartment(deparmentName)
        totalProfessors += d.getNumberOfProfessors()
    }
    fmt.Printf("Total number of professors in college2 is %d\n", totalProfessors)
}

func createCollege1() *college {
    college := &college{}
    college.addDepartment("computerscience", 4)
    college.addDepartment("mechanical", 5)
    return college
}

func createCollege2() *college {
    college := &college{}
    college.addDepartment("computerscience", 2)
    return college
} 
```

**college.go –** 表示学院。

```go
package main

type college struct {
    departments []department
}

func (c *college) addDepartment(departmentName string, numOfProfessors int) {
    if departmentName == "computerscience" {
        computerScienceDepartment := &computerscience{numberOfProfessors: numOfProfessors}
        c.departments = append(c.departments, computerScienceDepartment)
    }
    if departmentName == "mechanical" {
        mechanicalDepartment := &mechanical{numberOfProfessors: numOfProfessors}
        c.departments = append(c.departments, mechanicalDepartment)
    }
    return
}

func (c *college) getDepartment(departmentName string) department {
    for _, department := range c.departments {
        if department.getName() == departmentName {
            return department
        }
    }
    //Return a null department if the department doesn't exits
    return &nullDepartment{}
}
```

**department.go –** 它表示部门接口。

```go
package main

type department interface {
    getNumberOfProfessors() int
    getName() string
}
```

**computerscience.go –** 部门接口的具体实现。

```go
package main

type computerscience struct {
    numberOfProfessors int
}

func (c *computerscience) getNumberOfProfessors() int {
    return c.numberOfProfessors
}

func (c *computerscience) getName() string {
    return "computerscience"
}
```

**mechanical.go –** 部门接口的具体实现。

```go
package main

type mechanical struct {
    numberOfProfessors int
}

func (c *mechanical) getNumberOfProfessors() int {
    return c.numberOfProfessors
}

func (c *mechanical) getName() string {
    return "mechanical"
}
```

**nullDepartment.go** – 部门接口的 null 对象实现。

```go
package main

type nullDepartment struct {
    numberOfProfessors int
}

func (c *nullDepartment) getNumberOfProfessors() int {
    return 0
}

func (c *nullDepartment) getName() string {
    return "nullDepartment"
}
```

**输出：**

```go
Total number of professors in college1 is 9
Total number of professors in college2 is 2
```

## **完整工作代码：**

```go
package main

import "fmt"

type college struct {
    departments []department
}

func (c *college) addDepartment(departmentName string, numOfProfessors int) {
    if departmentName == "computerscience" {
        computerScienceDepartment := &computerscience{numberOfProfessors: numOfProfessors}
        c.departments = append(c.departments, computerScienceDepartment)
    }
    if departmentName == "mechanical" {
        mechanicalDepartment := &mechanical{numberOfProfessors: numOfProfessors}
        c.departments = append(c.departments, mechanicalDepartment)
    }
    return
}

func (c *college) getDepartment(departmentName string) department {
    for _, department := range c.departments {
        if department.getName() == departmentName {
            return department
        }
    }
    //Return a null department if the department doesn't exits
    return &nullDepartment{}
}

type department interface {
    getNumberOfProfessors() int
    getName() string
}

type computerscience struct {
    numberOfProfessors int
}

func (c *computerscience) getNumberOfProfessors() int {
    return c.numberOfProfessors
}

func (c *computerscience) getName() string {
    return "computerscience"
}

type mechanical struct {
    numberOfProfessors int
}

func (c *mechanical) getNumberOfProfessors() int {
    return c.numberOfProfessors
}

func (c *mechanical) getName() string {
    return "mechanical"
}

type nullDepartment struct {
    numberOfProfessors int
}

func (c *nullDepartment) getNumberOfProfessors() int {
    return 0
}

func (c *nullDepartment) getName() string {
    return "nullDepartment"
}

func main() {
    college1 := createCollege1()
    college2 := createCollege2()
    totalProfessors := 0
    departmentArray := []string{"computerscience", "mechanical", "civil", "electronics"}
    for _, deparmentName := range departmentArray {
        d := college1.getDepartment(deparmentName)
        totalProfessors += d.getNumberOfProfessors()
    }
    fmt.Printf("Total number of professors in college1 is %d\n", totalProfessors)
    totalProfessors = 0
    for _, deparmentName := range departmentArray {
        d := college2.getDepartment(deparmentName)
        totalProfessors += d.getNumberOfProfessors()
    }
    fmt.Printf("Total number of professors in college2 is %d\n", totalProfessors)
}

func createCollege1() *college {
    college := &college{}
    college.addDepartment("computerscience", 4)
    college.addDepartment("mechanical", 5)
    return college
}

func createCollege2() *college {
    college := &college{}
    college.addDepartment("computerscience", 2)
    return college
}
```

**输出：**

```go
Total number of professors in college1 is 9
Total number of professors in college2 is 2
```

+   [Golang](https://golangbyexample.com/tag/golang/)*   [Go 中的空对象设计模式](https://golangbyexample.com/tag/null-object-design-pattern-in-go/)*
