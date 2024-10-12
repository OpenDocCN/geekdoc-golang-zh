<!--yml
category: 未分类
date: 2024-10-13 06:20:31
-->

# Pointer Receiver for a method in Go (Golang)

> 来源：[https://golangbyexample.com/pointer-receiver-method-golang/](https://golangbyexample.com/pointer-receiver-method-golang/)

To better understand pointer receiver we first have to understand the value receiver for a method.

**Methods on Value Receiver**

Let’s see an example of a value receiver

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

Notice that the receiver is available inside the method and fields of the receiver can be accessed inside the method. Can field of the receiver also be changed inside the method?

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

In the above code a method **setNewName** is defined on the **employee** struct. In this method we update the name of the employee like this

```
e.name = newName
```

After setting the new name when we print the employee name again in the main function, we see that the old name “Sam” is printed instead of “John”. This happens because method is defined on a value receiver

```
func (e employee) setNewName(newName string) 
```

Since the method is defined on a value receiver when the method is called a copy of the receiver is made and that copy of the receiver is available inside the method. Since it is a copy, any changes made to the value receiver is not visible to the caller. That is why it prints the old name “Sam” instead of “John”. Now the question which comes to the mind whether there is any way to fix this. And the answer is yes, and this is where pointer receivers come into the picture.

**Method on a Pointer Receiver**

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

Is it necessary to create the employee pointer to call a method with a pointer receiver? No, it is not. The method can be called on the employee instance and the language will take care of it to correctly pass the receiver as a pointer to the method. This flexibility is provided by the language.

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

We see in above program that even if a method is defined on a pointer receiver but we are calling the method with a non-pointer employee instance

```
emp.setNewName("John")
```

But the language passes the receiver as a pointer and therefore the changes are visible to the caller.Even this way of calling is valid

```
(&emp).setNewName("Mike")
```

Now, how about the other way around. If a method is defined on a value receiver, can the method be called with a pointer of the receiver?

Yes, even this is valid and the language takes care of passing the argument correctly as value receiver irrespective of whether the method was called on a pointer or normal struct. Let’s see an example

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
To summarize what we learned above

*   If a method has a value receiver it supports calling of that method with both value and pointer receiver

*   If a method has a pointer receiver then also it supports calling of that method with both value and pointer receiver

This is unlike function where if

*   If a function has a pointer argument then it will only accept a pointer as an argument

*   If a function has a value argument then it will only accept a value as an argument

**When to use pointer receiver**

*   When the changes to the receiver made inside the method have to be visible to the caller.

*   When the struct is big, then it is better to use a pointer receiver otherwise a copy of the struct will be made every time a method is called which will be expensive

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)