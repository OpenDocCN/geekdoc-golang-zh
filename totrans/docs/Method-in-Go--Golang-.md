<!--yml
category: 未分类
date: 2024-10-13 06:20:25
-->

# Method in Go (Golang)

> 来源：[https://golangbyexample.com/method-in-golang/](https://golangbyexample.com/method-in-golang/)

This is the  chapter 20 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Interface](https://golangbyexample.com/interface-in-golang/)
**Previous Tutorial** – [Maps](https://golangbyexample.com/maps-in-golang)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Why Method](#Why_Method "Why Method")
*   [Format of a Method](#Format_of_a_Method "Format of a Method")
*   [Methods on Structs](#Methods_on_Structs "Methods on Structs")
*   [Method on a Pointer Receiver](#Method_on_a_Pointer_Receiver "Method on a Pointer Receiver")
*   [When to use pointer receiver](#When_to_use_pointer_receiver "When to use pointer receiver")
*   [Some More Points to note about methods](#Some_More_Points_to_note_about_methods "Some More Points to note about methods")
*   [Methods on Anonymous nested struct fields](#Methods_on_Anonymous_nested_struct_fields "Methods on Anonymous nested struct fields")
*   [Exported Method](#Exported_Method "Exported Method")
*   [Method Chaining](#Method_Chaining "Method Chaining")
*   [Methods on Non-Struct Types](#Methods_on_Non-Struct_Types "Methods on Non-Struct Types")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

A method in golang is nothing but a function with a receiver. A receiver is an instance of some specific type such as struct, but it can be an instance of any other custom type. So basically when you attach a function to a type, then that function becomes a method for that type. The method will have access to the properties of the receiver and can call the receiver’s other methods.

# **Why Method**

Since method lets you define a function on a type, it lets you write object-oriented code in Golang. There are also some other benefits such as two different methods can have the same name in the same package which is not possible with functions

# **Format of a Method**

Below is the format for a method

```
func (receiver receiver_type) some_func_name(arguments) return_values
```

The method receiver and receiver type appear between the **func** keyword and the function name. The return_values come at the last.

Also, let’s understand more differences between a function and a method. There are some important differences between them. Below is the signature of a function

**Function:**

```
func some_func_name(arguments) return_values
```

We have already seen the signature of a method

**Method:**

```
func (receiver receiver_type) some_func_name(arguments) return_values
```

From the above signature, it is clear that the method has a receiver argument. This is the only difference between function and method, but due to it they differ in terms of functionality they offer

*   A function can be used as first-order objects and can be passed around while methods cannot.

*   Methods can be used for chaining on the receiver while function cannot be used for the same.

*   There can exist different methods with the same name with a different receiver, but there cannot exist two different functions with the same name in the same package.

# **Methods on Structs**

Golang is not an object-oriented language. It doesn’t support type inheritance, but it does allow us to define methods on any custom type including structs. Since struct is a named collection of fields and methods can also be defined on it. As such struct in golang can be compared to a class in Object-Oriented Languages.

Let’s see an example of method on struct

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func (e employee) details() {
    fmt.Printf("Name: %s\n", e.name)
    fmt.Printf("Age: %d\n", e.age)
}

func (e employee) getSalary() int {
    return e.salary
}

func main() {
    emp := employee{name: "Sam", age: 31, salary: 2000}
    emp.details()
    fmt.Printf("Salary %d\n", emp.getSalary())
}
```

**Output**

```
Name: Sam
Age: 31
Salary 2000
```

Notice that the receiver is available inside the method and fields of the receiver can be accessed inside the method.

Can field of the receiver also be changed inside the method?

Let’s see that

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func (e employee) setNewName(newName string) {
    e.name = newName
}

func main() {
    emp := employee{name: "Sam", age: 31, salary: 2000}
    emp.setNewName("John")
    fmt.Printf("Name: %s\n", emp.name)
}
```

**Output**

```
Name: Sam
```

A method setNewName is defined on the employee struct in the above code. In this method, we update the name of the employee like this

```
e.name = newName
```

After setting the new name when we print the employee name again in the main function, we see that the old name “Sam” is printed instead of “John”. This happens because method is defined on a value receiver

```
func (e employee) setNewName(newName string)
```

Since the method is defined on a value receiver when the method is called a copy of the receiver is made and that copy of the receiver is available inside the method. Since it is a copy, any changes made to the value receiver is not visible to the caller. That is why it prints the old name “Sam” instead of “John”. Now the question which comes to the mind whether there is any way to fix this. And the answer is yes, and this is where pointer receivers come into the picture.

# **Method on a Pointer Receiver**

In the above example we saw a method on a value receiver. Any change made to a value receiver is not visible to the caller. Methods can also be defined on a pointer receiver. Any change made to the pointer receiver will be visible to the caller. Let’s see an example

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func (e *employee) setNewName(newName string) {
    e.name = newName
}

func main() {
    emp := &employee{name: "Sam", age: 31, salary: 2000}
    emp.setNewName("John")
    fmt.Printf("Name: %s\n", emp.name)
}
```

**Output**

```
Name: John
```

In above program, we defined the method **setNewName** on a pointer receiver

```
func (e *employee) setNewName(newName string)
```

Then we created an employee pointer and called the **setNewName** methodon it. We see that the changes made to the employee pointer inside the **setNewName** are visible to the caller and it prints the new name.

Is it necessary to create the employee pointer to call a method with a pointer receiver? No, it is not. The method can be called on the employee instance and the language will take care of it to correctly pass it as a pointer to the method. This flexibility is provided by the language.

Let’s see an example

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func (e *employee) setNewName(newName string) {
    e.name = newName
}

func main() {
    emp := employee{name: "Sam", age: 31, salary: 2000}
    emp.setNewName("John")

    fmt.Printf("Name: %s\n", emp.name)

    (&emp).setNewName("Mike")
    fmt.Printf("Name: %s\n", emp.name)
}
```

**Output**

```
Name: John
Name: Mike
```

We see in the above program that even if a method is defined on a pointer receiver but we are calling the method with a non-pointer employee instance

```
emp.setNewName("John")
```

But the language passes the receiver as a pointer and therefore the changes are visible to the caller.

Even this way of calling is valid

```
(&emp).setNewName("Mike")
```

Now, how about the other way around. If a method is defined on a value receiver, can the method be called with a pointer of the receiver?

Yes, even this is valid and the language takes care of passing the argument correctly as value receiver irrespective of whether the method was called on a pointer or normal struct.

Let’s see an example

```
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func (e employee) setNewName(newName string) {
    e.name = newName
}

func main() {
    emp := employee{name: "Sam", age: 31, salary: 2000}
    emp.setNewName("John")

    fmt.Printf("Name: %s\n", emp.name)
    (&emp).setNewName("Mike")

    fmt.Printf("Name: %s\n", emp.name)
    emp2 := &employee{name: "Sam", age: 31, salary: 2000}
    emp2.setNewName("John")
    fmt.Printf("Name: %s\n", emp2.name)
}
```

**Output**

```
Name: Sam
Name: Sam
Name: Sam
```

Do note here since in all three cases, the **setNewName** method had a value receiver hence changes are not visible to the caller as the value is passed as a copy. It prints the old name in all three cases

To summarize what we learnt above

*   If a method has a value receiver it supports calling of that method with both value and pointer receiver

*   If a method has a pointer receiver then also it supports calling of that method with both value and pointer receiver

This is unlike function where if

*   If a function has a pointer argument then it will only accept a pointer as an argument

*   If a function has a value argument then it will only accept a value as an argument

# **When to use pointer receiver**

*   When the changes to the receiver made inside the method have to be visible to the caller.

*   When the struct is big, then it is better to use a pointer receiver otherwise a copy of the struct will be made every time a method is called which will be expensive

# **Some More Points to note about methods**

*   The receiver type has to be defined in the same package as the method definition. On defining a method on a receiver that exists in a different package, below error will be raised.

```
ERROR: cannot define new methods on non-local types
```

*   Till now we have seen a method invocation using a dot operator. There is one other way to call a method as well as shown in below example

```
package main

import "fmt"

type employee struct {
	name   string
	age    int
	salary int
}

func (e employee) details() {
	fmt.Printf("Name: %s\n", e.name)
	fmt.Printf("Age: %d\n", e.age)
}

func (e *employee) setName(newName string) {
	e.name = newName
}

func main() {
	emp := employee{name: "Sam", age: 31, salary: 2000}
	employee.details(emp)

	(*employee).setName(&emp, "John")

	fmt.Printf("Name: %s\n", emp.name)
}
```

**Output**

```
Name: Sam
Age: 31
Name: John
```

In above example we see a different method for calling a method. There are two cases

*   When the method has a value receiver then it can be called as below which is struct name followed by method name. The first argument is the value receiver itself.

```
employee.details(emp)
```

*   When the method has a pointer receiver then it can be called as below which is a pointer to struct name followed by method name. The first argument is the pointer receiver.

```
(*employee).setName(&emp, "John")
```

Also note that arguments of the method starts from the second argument as for setName function above:

```
(*employee).setName(&emp, "John")
```

You will rarely see this style being used and the dot notation style that we discussed earlier is the recommended as well as the most common way.

# **Methods on Anonymous nested struct fields**

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

func (a address) details() {
	fmt.Printf("City: %s\n", a.city)
	fmt.Printf("Country: %s\n", a.country)
}

func main() {
	address := address{city: "London", country: "UK"}

	emp := employee{name: "Sam", age: 31, salary: 2000, address: address}

	emp.details()

	emp.address.details()
}
```

**Output**

```
City: London
Country: UK
City: London
Country: UK
```

Notice in above program that details method of address struct can be accessed in two ways

```
emp.details()
emp.address.details()
```

So in case of an anonymous nested struct, methods of that struct can be directly accessed.

# **Exported Method**

Go doesn’t have any public,  private or protected keyword. The only mechanism to control the visibility outside the package is using the capitalized and non-capitalized formats

*   **Capitalized Identifiers** are exported. The capital letter indicates that this is an exported identifier and is available outside the package.

*   **Non-capitalized identifiers **are not exported. The lowercase indicates that the identifier is not exported and will only be accessed from within the same package.

So any struct which starts with a capital letter is exported to other packages.  Similarly, any struct field which starts with capital is exported otherwise not. And also similarly any struct method which starts with a capital letter is exported. Let’s see an example that shows exporting and non-exporting of structs, struct fields, and methods.  See **model.go** and **test.go** below. Both belong to the **main** package.

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

**Output**

```
&{test 21}
&{}
test
21
21
test
```

If we move the file **model.go** to a different package named **model.** Now the output on running ‘go build’ will give compilation errors. All the compilation errors are because **test.go** in **main** package to not able to refer to un-exported fields of **model.go** in **model** package.

The compilation error will be

```
p.getName undefined (cannot refer to unexported field or method model.(*Person).getName)
```

# **Method Chaining**

For method chaining to be possible the methods in the chain should return the receiver. Returning the receiver for the last method inn the chain is optional.

Let’s see an example of method chaining.

```
package main

import "fmt"

type employee struct {
	name   string
	age    int
	salary int
}

func (e employee) printName() employee {
	fmt.Printf("Name: %s\n", e.name)
	return e
}

func (e employee) printAge() employee {
	fmt.Printf("Age: %d\n", e.age)
	return e
}

func (e employee) printSalary() {
	fmt.Printf("Salary: %d\n", e.salary)
}

func main() {
	emp := employee{name: "Sam", age: 31, salary: 2000}
	emp.printName().printAge().printSalary()
}
```

**Output**

```
Name: Sam
Age: 31
Salary: 2000
```

# **Methods on Non-Struct Types**

Methods can also be defined on a non-struct custom type. Non-struct custom types can be created through type definition. Below is the format for creating a new custom type

```
type {type_name} {built_in_type}
```

For example we can a named custom type **myFloat** of type **float64**

```
type myFloat float64
```

Methods can be defined on the named custom type. See below example:

**Code**

```
package main

import (
    "fmt"
    "math"
)

type myFloat float64

func (m myFloat) ceil() float64 {
    return math.Ceil(float64(m))
}

func main() {
    num := myFloat(1.4)
    fmt.Println(num.ceil())
}
```

**Output**

```
2
```

# **Conclusion**

This is all about using method in Go. Hope you have liked this article. Please share feedback/mistakes/improvements in comments

**Next Tutorial** – [Interface](https://golangbyexample.com/interface-in-golang/)
**Previous Tutorial** – [Maps](https://golangbyexample.com/maps-in-golang)

*   [complete guide](https://golangbyexample.com/tag/complete-guide/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*