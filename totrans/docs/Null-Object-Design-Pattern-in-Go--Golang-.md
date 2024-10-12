<!--yml
category: 未分类
date: 2024-10-13 06:03:37
-->

# Null Object Design Pattern in Go (Golang)

> 来源：[https://golangbyexample.com/null-object-design-pattern-golang/](https://golangbyexample.com/null-object-design-pattern-golang/)

Note: Interested in understanding how all other design patterns can be implemented in GO. Please see this full reference – [All Design Patterns in Go (Golang)](https://golangbyexample.com/all-design-patterns-golang/)

Table of Contents

 **   [Introduction:](#Introduction "Introduction:")
*   [Full Working Code:](#Full_Working_Code "Full Working Code:")*  *## **Introduction:**

Null Object Design Pattern is a behavioral design pattern. It is useful when the client code relies upon some dependency which can be null. Using this design pattern prevents clients from having to put null checks on the result of these dependencies. With that said, it should also be noted that client behavior is also fine with such null dependencies. 

Main components of the Null Object Design Pattern are:

1.  **Entity** – it is the interface which defines primitive operations that child structs have to implement
2.  **ConcreteEntity** – it implements the entity interface
3.  **NullEntity** – it represents the null object. It also implements the entity interface but has null properties
4.  **Client** – the client gets the implementation of the entity interface and uses it. It doesn’t really care if the implementation is of ConcreteEntity or NullEntity. It treats both of them as same.

Let’s consider an example. Assume we have a college with many departments with each department having some number of professors.

**department** is represented by an interface

```
type department interface {
    getNumberOfProfessors() int
    getName() string
}
```

where as **college** is represented as

```
type college struct {
    departments []department
}
```

Now let’s say that there is an agency that wants to calculate the total number of professors for a particular college for specific departments only.  We will use the Null Object Design Pattern for this use case where a college will return a nullDepartment object (see **nullDepartment.go**) if a department doesn’t exists in college.
Notice the code in agency.go

*   **agency.go** doesn’t even care whether a particular department exists in college or not. **college** returns a null department object if that department doesn’t exist in the college.
*   It treats nullDepartment and real Department as same so null checks are avoided. It calls **getNumberOfProfessors()** on both the objects.

Above are two advantages that we get by using nullObject design pattern for this case. See the below code

**agency.go**

```
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

**college.go –** represents the college

```
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

**department.go –** It represents the department interface

```
package main

type department interface {
    getNumberOfProfessors() int
    getName() string
}
```

**computerscience.go –** Concrete implementation of department interface

```
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

**mechanical.go –** Concrete implementation of department interface

```
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

**nullDepartment.go** – nullObject implementation of the department interface

```
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

**Output:**

```
Total number of professors in college1 is 9
Total number of professors in college2 is 2
```

## **Full Working Code:**

```
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

**Output:**

```
Total number of professors in college1 is 9
Total number of professors in college2 is 2
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [null object design pattern in go](https://golangbyexample.com/tag/null-object-design-pattern-in-go/)*