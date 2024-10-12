<!--yml
category: 未分类
date: 2024-10-13 06:03:22
-->

# Encapsulation in Go (Golang)

> 来源：[https://golangbyexample.com/encapsulation-in-go/](https://golangbyexample.com/encapsulation-in-go/)

Golang provides encapsulation at the package level. Go doesn’t have any public,  private or protected keyword. The only mechanism to control the visibility is using the capitalized and non-capitalized formats

*   **Capitalized Identifiers** are exported. The capital letter indicates that this is an exported identifier.
*   **Non-capitalized identifiers** are not exported. The lowercase indicates that the identifier is not exported and will only be accessed from within the same package.

There are five kinds of identifier which can be exported or non-exported

1.  Structure
2.  Structure’s Method
3.  Structure’s Field
4.  Function
5.  Variable

Let’s see an example that shows exporting and non-exporting of all the above identifiers. See **data.go** below. The package is **model**

*   Structure
    *   Struct **Person** is exported
    *   Struct **company** is non-exported
*   Structure’s Method
    *   **Person** Struct’s Method **GetAge()** is exported
    *   **Person** Struct’s Method **getName()** is not exported
*   Structure’s Field
    *   **Person** struct field **Name** is exported
    *   **Person** struct field **age** is not exported
*   Function
    *   Function **GetPerson()** is exported
    *   Function **getCompanyName()** is not exported
*   Variables
    *   Variable **CompanyName** is exported
    *   Variable **companyLocation** is not exported

**data.go**

```
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

Let’s write a file **test.go** in **model** package. See below.

**test.go**

```
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

On running this file, it is able to access all exported and unexported fields in **data.go** as both lies in the same package model. There is no compilation error and it gives below output

**Output:**

```
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

Let’s move the above file **test.go** to a different package named **view.** Now notice the output on running ‘go build’. It gives compilation errors. All the compilation error are due to not able to refer to unexported fields

**test.go**

```
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

**Output:**

```
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

*   [encapsulation](https://golangbyexample.com/tag/encapsulation/)*   [encapsulation in golang](https://golangbyexample.com/tag/encapsulation-in-golang/)*   [object oriented go](https://golangbyexample.com/tag/object-oriented-go/)*   [oop](https://golangbyexample.com/tag/oop/)*   [oop in go](https://golangbyexample.com/tag/oop-in-go/)