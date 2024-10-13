<!--yml

类别：未分类

日期：2024-10-13 06:20:25

-->

# Go（Golang）中的方法

> 来源：[https://golangbyexample.com/method-in-golang/](https://golangbyexample.com/method-in-golang/)

这是Golang综合教程系列的第20章。有关该系列其他章节的信息，请参考这个链接 – [Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [接口](https://golangbyexample.com/interface-in-golang/)

**上一个教程** – [地图](https://golangbyexample.com/maps-in-golang)

现在让我们查看当前教程。以下是当前教程的目录。

目录

**[概述](#Overview "概述")

+   [为什么使用方法](#Why_Method "为什么使用方法")

+   [方法的格式](#Format_of_a_Method "方法的格式")

+   [结构体上的方法](#Methods_on_Structs "结构体上的方法")

+   [指针接收者上的方法](#Method_on_a_Pointer_Receiver "指针接收者上的方法")

+   [何时使用指针接收者](#When_to_use_pointer_receiver "何时使用指针接收者")

+   [关于方法的更多注意事项](#Some_More_Points_to_note_about_methods "关于方法的更多注意事项")

+   [匿名嵌套结构字段上的方法](#Methods_on_Anonymous_nested_struct_fields "匿名嵌套结构字段上的方法")

+   [导出方法](#Exported_Method "导出方法")

+   [方法链](#Method_Chaining "方法链")

+   [非结构类型的方法](#Methods_on_Non-Struct_Types "非结构类型的方法")

+   [结论](#Conclusion "结论") *  *# **概述**

在Golang中，方法实际上就是一个带接收者的函数。接收者是某种特定类型的实例，如结构体，但它可以是任何其他自定义类型的实例。因此，当你将一个函数附加到一个类型时，这个函数就成为该类型的方法。方法将可以访问接收者的属性，并可以调用接收者的其他方法。

# **为什么使用方法**

由于方法允许你在类型上定义函数，因此它使你能够在Golang中编写面向对象的代码。此外，还有一些其他好处，比如在同一个包中两个不同的方法可以拥有相同的名称，而这在函数中是不可能的。

# **方法的格式**

以下是方法的格式

```go
func (receiver receiver_type) some_func_name(arguments) return_values
```

方法接收者和接收者类型出现在**func**关键字和函数名称之间。返回值出现在最后。

此外，让我们了解函数和方法之间的更多区别。它们之间有一些重要的区别。以下是函数的签名

**函数：**

```go
func some_func_name(arguments) return_values
```

我们已经看到了方法的签名

**方法：**

```go
func (receiver receiver_type) some_func_name(arguments) return_values
```

从上述签名中可以看出，方法有一个接收者参数。这是函数和方法之间唯一的区别，但由于这个区别，它们在功能上有所不同。

+   函数可以作为一等对象使用并可以传递，而方法则不能。

+   方法可以用于接收器上的链式调用，而函数不能以相同的方式使用。

+   可以存在不同接收器下同名的方法，但同一包内不能存在两个不同名称的函数。

# **结构体上的方法**

Golang 不是一种面向对象的语言。它不支持类型继承，但它允许我们在任何自定义类型（包括结构体）上定义方法。由于结构体是字段的命名集合，方法也可以在其上定义。因此，Golang 中的结构体可以与面向对象语言中的类进行比较。

让我们看一个结构体上方法的例子。

```go
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

**输出**

```go
Name: Sam
Age: 31
Salary 2000
```

请注意，接收器在方法内部是可用的，接收器的字段可以在方法内部访问。

接收器的字段也可以在方法内部更改吗？

让我们来看看。

```go
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

**输出**

```go
Name: Sam
```

在上面的代码中，定义了一个方法**setNewName**在员工结构体上。在这个方法中，我们这样更新员工的名字。

```go
e.name = newName
```

在设置新名字后，当我们在主函数中再次打印员工的名字时，我们看到打印的是旧名字“Sam”，而不是“John”。这是因为方法是在值接收器上定义的。

```go
func (e employee) setNewName(newName string)
```

由于该方法是在值接收器上定义的，因此当调用该方法时，会创建接收器的副本，并且该副本在方法内部可用。由于它是一个副本，对值接收器所做的任何更改对调用者都是不可见的。这就是为什么它打印旧名字“Sam”，而不是“John”。现在，心中浮现出一个问题，是否有任何方法可以解决这个问题。答案是肯定的，这就是指针接收器的作用所在。

# **指针接收器上的方法**

在上面的例子中，我们看到了一种关于值接收器的方法。对值接收器所做的任何更改对调用者都是不可见的。方法也可以在指针接收器上定义。对指针接收器所做的任何更改将对调用者可见。让我们看一个例子。

```go
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

**输出**

```go
Name: John
```

在上面的程序中，我们在指针接收器上定义了方法**setNewName**。

```go
func (e *employee) setNewName(newName string)
```

然后我们创建了一个员工指针并在其上调用了**setNewName**方法。我们看到在**setNewName**内部对员工指针所做的更改对调用者是可见的，并且打印出新的名字。

调用一个具有指针接收器的方法是否有必要创建员工指针？不，没必要。可以在员工实例上调用该方法，语言会处理正确将其作为指针传递给方法。这种灵活性是语言提供的。

让我们看一个例子。

```go
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

**输出**

```go
Name: John
Name: Mike
```

我们在上面的程序中看到，即使方法是在指针接收器上定义的，但我们用非指针的员工实例来调用这个方法。

```go
emp.setNewName("John")
```

但语言将接收器作为指针传递，因此更改对调用者是可见的。

这种调用方式也是有效的。

```go
(&emp).setNewName("Mike")
```

现在，另一种情况。如果在值接收者上定义了一个方法，可以用接收者的指针调用该方法吗？

是的，即使这样也是有效的，语言会确保正确地将参数作为值接收者传递，无论该方法是在指针或普通结构上调用的。

让我们看一个示例。

```go
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

**输出**

```go
Name: Sam
Name: Sam
Name: Sam
```

请注意，在这三种情况下，**setNewName**方法有一个值接收者，因此更改对调用者不可见，因为值作为副本传递。它在所有三种情况下都打印旧名称。

总结一下我们上面学到的内容。

+   如果方法有一个值接收者，它支持用值接收者和指针接收者调用该方法。

+   如果方法有一个指针接收者，则也支持用值接收者和指针接收者调用该方法。

这与函数不同，如果

+   如果函数有一个指针参数，则它只会接受一个指针作为参数。

+   如果函数有一个值参数，则它只会接受一个值作为参数。

# **何时使用指针接收者**

+   当方法内部对接收者的更改需要对调用者可见时。

+   当结构体很大时，最好使用指针接收者，否则每次调用方法时都会生成结构体的副本，这样开销很大。

# **关于方法的更多注意事项**

+   接收者类型必须在与方法定义相同的包中定义。在定义一个存在于不同包中的接收者上的方法时，将会引发以下错误。

```go
ERROR: cannot define new methods on non-local types
```

+   到目前为止，我们已经看到使用点操作符进行方法调用的方法。还有另一种调用方法的方式，如下面的示例所示。

```go
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

**输出**

```go
Name: Sam
Age: 31
Name: John
```

在上面的示例中，我们看到了一种不同的方法来调用方法。有两种情况。

+   当方法有一个值接收者时，可以如下调用，即结构体名称后跟方法名称。第一个参数是值接收者本身。

```go
employee.details(emp)
```

+   当方法有一个指针接收者时，可以如下调用，即指向结构体名称的指针后跟方法名称。第一个参数是指针接收者。

```go
(*employee).setName(&emp, "John")
```

还需注意，方法的参数从第二个参数开始，正如上面setName函数所示：

```go
(*employee).setName(&emp, "John")
```

这种风格很少被使用，而我们之前讨论的点表示法是推荐的，也是最常见的方式。

# **匿名嵌套结构字段上的方法**

让我们看一个程序。

```go
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

**输出**

```go
City: London
Country: UK
City: London
Country: UK
```

请注意，在上面的程序中，地址结构的details方法可以通过两种方式访问。

```go
emp.details()
emp.address.details()
```

因此，在匿名嵌套结构的情况下，可以直接访问该结构的方法。

# **导出方法**

Go没有任何公共、私有或保护关键字。控制包外可见性的唯一机制是使用大写和小写格式。

+   **大写标识符**是导出的。大写字母表示这是一个导出标识符，可以在包外使用。

+   **非首字母大写的标识符** 是非导出的。小写字母表示该标识符是非导出的，只能从同一包内访问。

因此，任何以大写字母开头的结构体都是导出的。同样，任何以大写字母开头的结构体字段也是导出的，反之亦然。任何以大写字母开头的结构体方法也是导出的。让我们看一个示例，展示结构体、结构体字段和方法的导出与非导出。见下面的 **model.go** 和 **test.go**。它们都属于 **main** 包。

+   结构

    +   结构体 **Person** 是导出的

    +   结构体 **company** 是非导出的

+   结构的字段

    +   **Person** 结构体字段 **Name** 是导出的

    +   **Person** 结构体字段 **age** 是非导出的，但 **Name** 是导出的

+   结构的方法

    +   **Person** 结构体的方法 **GetAge()** 是导出的

    +   **Person** 结构体的方法 **getName()** 是非导出的

**model.go**

```go
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

让我们在同一个 **main** 包中编写一个文件 **test.go**。见下文。

**test.go**

```go
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

运行此文件时，它能够访问 **model.go** 中所有导出和非导出的字段，因为它们都在同一个 **main** 包中。没有编译错误，输出如下

**输出**

```go
&{test 21}
&{}
test
21
21
test
```

如果我们将文件 **model.go** 移动到一个名为 **model** 的不同包中，那么在运行 ‘go build’ 时会出现编译错误。所有编译错误都是因为 **test.go** 在 **main** 包中无法引用 **model.go** 中的非导出字段。

编译错误将是

```go
p.getName undefined (cannot refer to unexported field or method model.(*Person).getName)
```

# **方法链**

为了使方法链成为可能，链中的方法应返回接收者。链中最后一个方法返回接收者是可选的。

让我们看看方法链的一个例子。

```go
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

**输出**

```go
Name: Sam
Age: 31
Salary: 2000
```

# **非结构类型的方法**

方法也可以在非结构自定义类型上定义。非结构自定义类型可以通过类型定义创建。以下是创建新自定义类型的格式

```go
type {type_name} {built_in_type}
```

例如，我们可以创建一个名为 **myFloat** 的自定义类型，其类型为 **float64**

```go
type myFloat float64
```

方法可以在命名的自定义类型上定义。请参见下面的示例：

**代码**

```go
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

**输出**

```go
2
```

# **结论**

这就是关于在 Go 中使用方法的全部内容。希望你喜欢这篇文章。请在评论中分享反馈/错误/改进建议

**下一个教程** – [接口](https://golangbyexample.com/interface-in-golang/)

**前一个教程** – [地图](https://golangbyexample.com/maps-in-golang)

+   [完整指南](https://golangbyexample.com/tag/complete-guide/) * [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/) *
