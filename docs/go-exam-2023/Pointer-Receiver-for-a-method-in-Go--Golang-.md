<!--yml

类别：未分类

日期：2024-10-13 06:20:31

-->

# Go（Golang）中的指针接收者方法

> 来源：[`golangbyexample.com/pointer-receiver-method-golang/`](https://golangbyexample.com/pointer-receiver-method-golang/)

为了更好地理解指针接收者，我们首先必须理解方法的值接收者。

**值接收者上的方法**

让我们看看值接收者的一个例子。

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

注意，接收者在方法内部是可用的，接收者的字段可以在方法内部访问。接收者的字段也可以在方法内部更改吗？

让我们看看这个。

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

在上述代码中，定义了一个方法**setNewName**，它在**employee**结构体上。在这个方法中，我们像这样更新员工的名字。

```go
e.name = newName
```

在设置新名字后，当我们在主函数中再次打印员工名字时，我们看到打印的是旧名字“Sam”，而不是“John”。这是因为方法是在值接收者上定义的。

```go
func (e employee) setNewName(newName string) 
```

由于方法是在值接收者上定义的，当调用该方法时，会生成接收者的副本，该副本在方法内部可用。由于这是一个副本，对值接收者所做的任何更改对调用者都是不可见的。这就是为什么它打印旧名字“Sam”而不是“John”。现在，脑海中产生的问题是是否有办法修复这个问题。答案是有，这就是指针接收者进入的场景。

**指针接收者上的方法**

在上述示例中，我们看到了一个值接收者的方法。对值接收者所做的任何更改对调用者不可见。方法也可以在指针接收者上定义。对指针接收者所做的任何更改对调用者是可见的。让我们来看一个例子。

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

在上面的程序中，我们在指针接收者上定义了方法**setNewName**。

```go
func (e *employee) setNewName(newName string)
```

然后我们创建了一个员工指针，并在其上调用了**setNewName**方法。我们看到在**setNewName**内部对员工指针所做的更改对调用者是可见的，并且打印了新名字。

是否有必要创建员工指针以调用带有指针接收者的方法？不，没有必要。可以在员工实例上调用该方法，语言会自动将接收者作为指针传递给方法。这种灵活性是语言提供的。

让我们看看一个例子。

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

我们在上面的程序中看到，即使方法在指针接收者上定义，但我们仍然使用非指针的员工实例调用该方法。

```go
emp.setNewName("John")
```

但是语言将接收者作为指针传递，因此更改对调用者是可见的。即使这样调用也是有效的。

```go
(&emp).setNewName("Mike")
```

现在，反过来，如果一个方法在值接收者上定义，可以用接收者的指针来调用该方法吗？

是的，这也是有效的，语言会确保正确地将参数作为值接收者传递，无论方法是调用在指针还是普通结构上。让我们看看一个例子。

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

请注意，在这三种情况下，**setNewName** 方法具有值接收者，因此更改对调用者不可见，因为值是作为副本传递的。在这三种情况下，它都打印旧名称。

总结一下我们上面学到的内容：

+   如果一个方法有一个值接收者，它支持用值和指针接收者来调用该方法。

+   如果方法有一个指针接收者，那么它也支持使用值和指针接收者来调用该方法。

这与函数不同，如果

+   如果一个函数有一个指针参数，那么它只会接受指针作为参数。

+   如果一个函数有一个值参数，那么它只会接受值作为参数。

**何时使用指针接收者**

+   当方法内部对接收者所做的更改必须对调用者可见时。

+   当结构体较大时，最好使用指针接收者，否则每次调用方法时都会创建结构体的副本，这将非常耗费资源。

+   [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/)
