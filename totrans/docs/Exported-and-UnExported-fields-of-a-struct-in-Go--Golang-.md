<!--yml
category: 未分类
date: 2024-10-13 06:19:34
-->

# Exported and UnExported fields of a struct in Go (Golang)

> 来源：[https://golangbyexample.com/exported-unexported-fields-struct-go/](https://golangbyexample.com/exported-unexported-fields-struct-go/)

Go doesn’t have any public,  private or protected keyword. The only mechanism to control the visibility outside the package is using the capitalized and non-capitalized formats

*   **Capitalized Identifiers** are exported. The capital letter indicates that this is an exported identifier and is available outside the package.

*   **Non-capitalized identifiers **are not exported. The lowercase indicates that the identifier is not exported and will only be accessed from within the same package.

So any struct which starts with a capital letter is exported to other packages.  Similarly, any struct field which starts with capital is exported otherwise not. And also similarly any struct method which starts with a capital letter is exported. Let’s see an example that shows exporting and non-exporting of structs, struct fields, and methods. See **model.go** and **test.go** below. Both belong to the **main** package.

*   Structure
    *   Struct **Person** is exported
    *   Struct **company** is non-exported

*   Structure’s Field
    *   **Person** struct field **Name** is exported
    *   **Person** struct field **age** is not exported but **Name** is exported

*   Structure’s Method
    *   **Person** Struct’s Method **GetAge()** is exported
    *   **Person** Struct’s Method **getName()** is not exported

**model.go**

```
package main

import "fmt"

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
```

Let’s write a file **test.go** in same **main** package. See below.

**test.go**

```
package main

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

    //STRUCTURE'S FIELDS
    fmt.Println(p.Name)
    fmt.Println(p.age)

    //STRUCTURE'S METHOD
    fmt.Println(p.GetAge())
    fmt.Println(p.getName())

}
```

On running this file, it is able to access all exported and un-exported fields in **model.go** as both lies in the same package **main**. There is no compilation error and it gives below output

**Output:**

```
&{test 21}
&{}
test
21
21
test
```

Let’s move the above file **model.go** to a different package named **model.** Now notice the output on running ‘go build’. It gives compilation errors. All the compilation error are because **test.go** in **main** package to not able to refer to un-exported fields of **model.go** in **model** package

**model.go**

```
package model

//Person struct
type Person struct {
	Name string
	age  int
}

func (p *Person) GetAge() int {
    return p.age
}

func (p *Person) getName() string {
    return p.Name
}

type company struct {
}
```

**test.go**

```
package main

import (
	"fmt"
        //This will path of your model package
	"<somepath>/model"
)

//Test function
func main() {
	//STRUCTURE IDENTIFIER
	p := &model.Person{
		Name: "test",
		age:  21,
	}
	fmt.Println(p)
	c := &model.company{}
	fmt.Println(c)

	//STRUCTURE'S FIELDS
	fmt.Println(p.Name)
	fmt.Println(p.age)

        //STRUCTURE'S METHOD
        fmt.Println(p.GetAge())
        fmt.Println(p.getName())

}</somepath>
```

**Output:**

```
unknown field 'age' in struct literal of type model.Person
cannot refer to unexported name model.company
undefined: model.company
p.age undefined (cannot refer to unexported field or method age)
p.getName undefined (cannot refer to unexported field or method model.(*Person).getName)
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)