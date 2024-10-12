<!--yml
category: 未分类
date: 2024-10-13 06:19:44
-->

# Struct in Go (Golang)

> 来源：[https://golangbyexample.com/struct-in-golang-complete-guide/](https://golangbyexample.com/struct-in-golang-complete-guide/)

This is the  chapter 16 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Array](https://golangbyexample.com/understanding-array-golang-complete-guid)
**Previous Tutorial** – [Pointer](https://golangbyexample.com/pointer-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Declaring a struct type](#Declaring_a_struct_type "Declaring a struct type")
*   [Creating a struct variable](#Creating_a_struct_variable "Creating a struct variable")
*   [Accessing and Setting Struct Fields](#Accessing_and_Setting_Struct_Fields "Accessing and Setting Struct Fields")
*   [Pointer to a struct](#Pointer_to_a_struct "Pointer to a struct")
    *   [Using the & operator](#Using_the_operator "Using the & operator")
    *   [Using the new keyword](#Using_the_new_keyword "Using the new keyword")
*   [Print a Struct Variable](#Print_a_Struct_Variable "Print a Struct Variable")
    *   [Using the fmt package](#Using_the_fmt_package "Using the fmt package")
    *   [Printing the struct in JSON form](#Printing_the_struct_in_JSON_form "Printing the struct in JSON form")
*   [Struct Field Meta or Tags](#Struct_Field_Meta_or_Tags "Struct Field Meta or Tags")
*   [Anonymous Fields in a Struct](#Anonymous_Fields_in_a_Struct "Anonymous Fields in a Struct")
*   [Nested Struct](#Nested_Struct "Nested Struct")
*   [Anonymous nested struct fields](#Anonymous_nested_struct_fields "Anonymous nested struct fields")
*   [Exported and UnExported fields of a struct](#Exported_and_UnExported_fields_of_a_struct "Exported and UnExported fields of a struct")
*   [Struct Equality](#Struct_Equality "Struct Equality")
*   [Struct are value types](#Struct_are_value_types "Struct are value types")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

GO struct is named collection of data fields which can be of different types. Struct acts as a container that has different heterogeneous data types which together represents an entity. For example, different attributes are used to represent an employee in an organization. Employee can have

*   Name of string type

*   Age of int type

*   DOB of time.Time type

*   Salary of int type

.. and so on. A struct can be used to represent an employee

```
type employee struct {
    name   string
    age    int
    salary int
}
```

A struct in golang can be compared to a class in Object Oriented Languages

# **Declaring a struct type**

Below is the format for declaring a struct

```
type struct_name struct {
    field_name1 field_type1
    field_name2 field_type2
    ...
}
```

In the above format, **struct_name** is the name of the struct. It has a field named **field_name1** of type **field_type1** and a field named **field_name2** of type **field_type2**. This declares a new named struct type which acts as a blueprint. The type keyword is used to introduce a new type

Example

```
type point struct {
    x float64
    y float64
}
```

The above declaration declares a new struct named **point** which has two field **x** and **y**. Both fields are of **float64** type.Once a new struct type is declared we can define new concrete struct variable from it as we will see in next section

# **Creating a struct variable**

Declaring a struct just declares a named struct type. Creating a struct variable creates an instance of that struct with memory being initialized as well. We can create a empty struct variable without given any value to any of the field

```
emp := employee{}
```

In this case, all the fields in the struct are initialized with a default zero value of that field type.

We can also initialize the value for each struct field while creating a struct variable. There are two variations

*   Each field on the same line

```
emp := employee{name: "Sam", age: 31, salary: 2000}
```

*   Each field on different lines

```
emp := employee{
   name:   "Sam",
   age:    31,
   salary: 2000,
}
```

It is also ok to initialize only some of the fields with value. The field which are not initialized with value will get the default zero value of their type

```
emp := employee{
   name: "Sam",
   age: 31,
}
```

In above case salary will get default value of zero since it is not initialized

Let’s see a working code illustrating above points:

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

**Output**

```
Emp1: {name: age:0 salary:0}
Emp2: {name:Sam age:31 salary:2000}
Emp3: {name:Sam age:31 salary:2000}
Emp4: {name:Sam age:31 salary:0}
```

For above program

*   We first declare an **employee** struct.

*   emp1’s fields are all initialized with default zero value of its type i.e name with “”, age and salary with 0.

*   emp2 has been initialized with all fields on the same line. Its fields are correctly printed with their value

*   emp3’s has been initialized with all fields on different lines. Its fields are correctly printed with their value

*   emp4’s salary field is initialized with default zero value of 0\. While other other two fields are correctly printed with their value.

It is to be noted that in the initialization of a struct, every new line with in curly braces has to end with a comma. So below initialization will raise error as

```
"salary" : 2000
```

doesn’t end with a comma.

```
emp := employee{
  name:   "Sam",
  age:    31,
  salary: 2000
}
```

This will be fine

```
emp := employee{
  name:   "Sam",
  age:    31,
  salary: 2000}
```

**Without field names**

struct can also be initialized without specifying the field names. But in this case, all values for each of the field has to be provided in sequence

```
emp := employee{"Sam", 31, 2000}
```

A compiler error will be raised if all values are not provided when field name is not used.

Let’s see a program

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

**Output**

```
Emp2: {name:Sam age:31 salary:2000}
```

Uncomment the line

```
emp = employee{"Sam", 31}
```

in the above program, and it will raise compiler error

```
too few values in employee literal
```

# **Accessing and Setting Struct Fields**

Structs fields can be accessed using the dot operator. Below is the format for getting the value

```
n := emp.name
```

Similarly a value can be assigned to a struct field too.

```
emp.name = "some_new_name"
```

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

**Output**

```
Current name is: Sam
New name is: John
```

# **Pointer to a struct**

Thereare two ways of creating a pointer to the struct

*   Using the & operator

*   Using the new keyword

Let’s looks at each of above method one by one.

## **Using the & operator**

The **&** operator can be used to get the pointer to a struct variable.

```
emp := employee{name: "Sam", age: 31, salary: 2000}
empP := &emp
```

struct pointer can also be directly created as well

```
empP := &employee{name: "Sam", age: 31, salary: 2000}
```

Let’s look at a program

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
    empP := &emp
    fmt.Printf("Emp: %+v\n", empP)
    empP = &employee{name: "John", age: 30, salary: 3000}
    fmt.Printf("Emp: %+v\n", empP)
}
```

**Output**

```
Emp: &{name:Sam age:31 salary:2000}
Emp: &{name:John age:30 salary:3000}
```

## **Using the new keyword**

Using the  new() keyword will:

*   Create the struct

*   Initialize all the field to the zero default value of their type

*   Return the pointer to the newly created struct

This will return a pointer

```
empP := new(employee)
```

Pointer address can be print using the **%p** format modifier

```
fmt.Printf("Emp Pointer: %p\n", empP)
```

Deference operator ‘*’ can be used to print the value at the pointer.

```
fmt.Printf("Emp Value: %+v\n", *empP)
```

It will print

```
Emp Value: {name: age:0 salary:0}
```

When not using the dereference pointer but using the format identifier**%+v,** then ampersand will be appended before the struct indicating that is a pointer.

```
fmt.Printf("Emp Value: %+v\n", empP)
```

It will print

```
Emp Value: &{name: age:0 salary:0}
```

Let’s see full program denoting above points

```
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

**Output**

```
Emp Pointer Address: 0xc000130000
Emp Pointer: &{name: age:0 salary:0}
Emp Value: {name: age:0 salary:0}
```

# **Print a Struct Variable**

Thereare two ways to print all struct variables including all its key and values.

*   Using the **fmt** package

*   Printing the struct in JSON form using the **json/encoding** package. This also allows pretty print of a struct as well.

Let’s see the two ways in which we can print the instance of the employee struct.

## **Using the fmt package**

**fmt.Printf()** function can be used to print a struct.  Different format identifiers can be used to print a struct in different ways. Let’s see how different format identifiers can be used to print a struct in different formats.

Let’s first create an instance of employee

```
emp := employee{name: "Sam", age: 31, salary: 2000}
```

*   **%v** – It will print only values. Field name will not be printed. This is the default way of printing a struct. Eg

```
fmt.Printf("%v", emp)  -  {Sam 31 2000}
```

*   **%+v –** It will print both field and value. Eg

```
fmt.Printf("%+v", emp) - {name:Sam age:31 salary:2000}
```

**fmt.Println()** function can also be used to print a struct. Since %v is the default for **fmt.Printlin()** function, hence output will be same as using %v for **fmt.Printf()**

```
fmt.Println(emp) - {Sam 31 2000}
```

Let’s see a working program too

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
    fmt.Printf("Emp: %v\n", emp)
    fmt.Printf("Emp: %+v\n", emp)
    fmt.Printf("Emp: %#v\n", emp)
    fmt.Println(emp)
}
```

**Output**

```
Emp: {Sam 31 2000}
Emp: {name:Sam age:31 salary:2000}
Emp: main.employee{name:"Sam", age:31, salary:2000}
{Sam 31 2000}
```

## **Printing the struct in JSON form**

Second method is to print the struct in the JSON format. **Marshal** and **MarshalIndent** function of **encoding/json** package can be used to print a struct in JSON format. Here is the difference

*   **Marshal** – Below is the signature of the **Marshal** function. This function returns the JSON encoding of **v** by traversing the value recursively

```
Marshal(v interface{}) ([]byte, error)
```

*   **MarshalIndent**– Below is the signature of the **MarshalIndent** function. It is similar to **Marshal** function but applies Indent to format the output. So it can be used to pretty print a struct

```
MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
```

It is to be noted that both **Marshal** and **MarshalIndent** function can only access the exported fields of a struct, which means that only the capitalized fields can be accessed and encoded in JSON form.

```
package main

import (
    "encoding/json"
    "fmt"
    "log"
)

type employee struct {
    Name   string
    Age    int
    salary int
}

func main() {
    emp := employee{Name: "Sam", Age: 31, salary: 2000}
    //Marshal
    empJSON, err := json.Marshal(emp)
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Marshal funnction output %s\n", string(empJSON))

    //MarshalIndent
    empJSON, err = json.MarshalIndent(emp, "", "  ")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("MarshalIndent funnction output %s\n", string(empJSON))
}
```

**Output:**

```
Marshal funnction output {"Name":"Sam","Age":31}

MarshalIndent funnction output {
  "Name": "Sam",
  "Age": 31
}
```

The salary field is not printed in the output because it begins with a lowercase letter and is not exported. The **Marshal** function output is not formatted while the **MarshalIndent** function output is formatted.

**golang** also allows the JSON encoded struct key name to be different by the use of struct meta fields as will see in the next section.

# **Struct Field Meta or Tags**

A struct in go also allows adding metadata to its fields. These meta fields can be used to encode decode into different forms, doing some forms of validations on struct fields, etc. So basically any meta information can be stored with fields of a struct and can be used by any package or library for different purposes.

Below is the format for attaching a meta-data. Meta-data is a string literal i.e it is enclosed in backquotes

```
type strutName struct{
   fieldName type `key:value key2:value2`
}
```

Now for our use case, we will add JSON tags to employee struct as below. Marshal function will use the key name specified in the tags

```
type employee struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    Salary int    `json:"s"`
}
```

Let’s see full program

```
package main

import (
    "encoding/json"
    "fmt"
    "log"
)

type employee struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    Salary int    `json:"s"`
}

func main() {
    emp := employee{Name: "Sam", Age: 31, Salary: 2000}
    //Converting to jsonn
    empJSON, err := json.MarshalIndent(emp, '', '  ')
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Println(string(empJSON))
}
```

**Output:**

```
{
  "n": "Sam",
  "a": 31,
  "s": 2000
}
```

The key name in the output is same as specified in the json meta tags.

# **Anonymous Fields in a Struct**

Astruct can have anonymous fields as well, meaning a field having no name. The type will become the field name. In below example, **string** will be the field name as well

```
type employee struct {
    string
    age    int
    salary int
}
```

The anonymous field can also be accessed and assigned a value

```
package main

import "fmt"

type employee struct {
    string
    age    int
    salary int
}

func main() {
    emp := employee{string: "Sam", age: 31, salary: 2000}
    //Accessing a struct field
    n := emp.string
    fmt.Printf("Current name is: %s\n", n)
    //Assigning a new value
    emp.string = "John"
    fmt.Printf("New name is: %s\n", emp.string)
}
```

**Output**

```
Current name is: Sam
New name is: John
```

# **Nested Struct**

A struct can have another struct nested in it. Let’s see an example of a nested struct. In below **employee** struct has **address** struct nested it in.

```
package main

import "fmt"

type employee struct {
    name    string
    age     int
    salary  int
    address address
}

type address struct {
    city    string
    country string
}

func main() {
    address := address{city: "London", country: "UK"}
    emp := employee{name: "Sam", age: 31, salary: 2000, address: address}
    fmt.Printf("City: %s\n", emp.address.city)
    fmt.Printf("Country: %s\n", emp.address.country)
}
```

**Output**

```
City: London
Country: UK
```

Notice how nested struct fields are accessed.

```
emp.address.city
emp.address.country
```

# **Anonymous nested struct fields**

The nested struct field can also be anonymous. Also, in this case, nested struct’s fields are directly accessed. So below is valid

```
emp.city
emp.country
```

It is also to be noted that below is still valid in this case

```
emp.address.city
emp.address.country
```

Let’s see a program

```
package main

import "fmt"

type employee struct {
	name   string
	age    int
	salary int
	address
}

type address struct {
	city    string
	country string
}

func main() {
	address := address{city: "London", country: "UK"}

	emp := employee{name: "Sam", age: 31, salary: 2000, address: address}

	fmt.Printf("City: %s\n", emp.address.city)
	fmt.Printf("Country: %s\n", emp.address.country)

	fmt.Printf("City: %s\n", emp.city)
	fmt.Printf("Country: %s\n", emp.country)
}
```

**Output**

```
City: London
Country: UK
City: London
Country: UK
```

Notice in above program that city field of address struct can be accessed in two ways

```
emp.city
emp.address.city
```

Similar for the country field of the address struct.

# **Exported and UnExported fields of a struct**

Go doesn’t have any public,  private or protected keyword. The only mechanism to control the visibility outside the package is using the capitalized and non-capitalized formats

*   **Capitalized Identifiers** are exported. The capital letter indicates that this is an exported identifier and is available outside the package.

*   **Non-capitalized identifiers **are not exported. The lowercase indicates that the identifier is not exported and will only be accessed from within the same package.

So any struct which starts with a capital letter is exported to other packages.  Similarly any struct field which starts with capital is exported otherwise not. Let’s see an example that shows exporting and non-exporting of structs and struct fields. See **model.go** and **test.go** below. Both belong to the **main** package.

*   Structure
    *   Struct **Person** is exported
    *   Struct **company** is non-exported

*   Structure’s Field
    *   **Person** struct field **Name** is exported
    *   **Person** struct field **age** is not exported but **Name** is exported

**model.go**

```
package main

import "fmt"

//Person struct
type Person struct {
    Name string
    age  int
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
}
```

On running this file, it is able to access all exported and un-exported fields in **model.go** as both lies in the same package **main**. There is no compilation error and it gives below output

**Output:**

```
&{test 21}
&{}
test
21
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
}</somepath>
```

**Output:**

```
cannot refer to unexported name model.company
p.age undefined (cannot refer to unexported field or method age)
```

# **Struct Equality**

The first thing to know before considering struct equality is weather if all struct fields types are comparable or not

Some of the comparable types as defined by go specification are

*   boolean
*   numeric
*   string,
*   pointer
*   channel
*   interface types
*   structs – if all it’s field type is comparable
*   array – if the type of value of array element is comparable

Some of the types which are not comparable as per go specification and which cannot be used as a key in a map are.

*   Slice
*   Map
*   Function

So two struct will be equal if first all their field types are comparable and all the corresponding field values are equal. Let’s see an example

```
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

**Output**

```
emp1 annd emp2 are equal
```

If the struct field type are not comparable then there will be compilation error on checking struct equality using the == operator.

```
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

Above program will raise compilation error as **employee** struct contains a field **deparments** which is a **slice** of **string**. **slice** is not a comparable type and hence the compilation error.

```
invalid operation: emp1 == emp2 (struct containing []string cannot be compared)
```

# **Struct are value types**

A struct is value type in go. So a struct variable name is not a pointer to the struct in fact it denotes the entire struct. A new copy of the struct will be created when

*   A struct variable is assigned to another struct variable.

*   A struct variable is passed as an argument to a function.

Let’s see above point with another example

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func main() {
    emp1 := employee{name: "Sam", age: 31, salary: 2000}
    fmt.Printf("Emp1 Before: %v\n", emp1)

    emp2 := emp1

    emp2.name = "John"
    fmt.Printf("Emp1 After assignment: %v\n", emp1)
    fmt.Printf("Emp2: %v\n", emp2)

    test(emp1)
    fmt.Printf("Emp1 After Test Function Call: %v\n", emp1)
}

func test(emp employee) {
    emp.name = "Mike"
    fmt.Printf("Emp in Test function: %v\n", emp)
}
```

**Output**

```
Emp1 Before: {Sam 31 2000}
Emp1 After assignment: {Sam 31 2000}
Emp2: {John 31 2000}
Emp in Test function: {Mike 31 2000}
Emp1 After Test Function Call: {Sam 31 2000}
```

In above example,

*   we assigned the **emp1** to **emp2** and we then changed name  **emp2** to have a different value.  After that when we print **emp1**, we see that it hasn’t changed. This is because when we assign **emp1** to **emp2**, a copy is created and changing **emp2** doesn’t have any effect on **emp1**

*   We passed **emp1** to the test function and then again changed its **name** field in the test function.  After that when we print **emp1**, we see that it hasn’t changed. The reason is same, when **emp1** is passed as an argument to test function a copy of **emp1** is created.

# **Conclusion**

This is all about struct in golang. In this article, we learned different ways of initializing a struct, pointer to struct, different ways of printing, about anonymous fields, etc. I hope you have liked this article. Please share the feedback/improvements/mistakes in the comments.

**Next Tutorial** – [Array](https://golangbyexample.com/understanding-array-golang-complete-guid)
**Previous Tutorial** – [Pointer](https://golangbyexample.com/pointer-golang/)

*   [complete guide](https://golangbyexample.com/tag/complete-guide/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*