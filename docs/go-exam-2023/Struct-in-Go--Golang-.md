<!--yml

分类：未分类

日期：2024-10-13 06:19:44

-->

# Go（Golang）中的结构

> 来源：[https://golangbyexample.com/struct-in-golang-complete-guide/](https://golangbyexample.com/struct-in-golang-complete-guide/)

这是 golang 综合教程系列的第 16 章。有关系列其他章节，请参阅此链接 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一教程** – [数组](https://golangbyexample.com/understanding-array-golang-complete-guid)

**上一个教程** – [指针](https://golangbyexample.com/pointer-golang/)

现在让我们查看当前的教程。以下是当前教程的目录

目录

**[概述](#Overview "概述")

+   [声明一个结构类型](#Declaring_a_struct_type "声明一个结构类型")

+   [创建一个结构变量](#Creating_a_struct_variable "创建一个结构变量")

+   [访问和设置结构字段](#Accessing_and_Setting_Struct_Fields "访问和设置结构字段")

+   [指向结构的指针](#Pointer_to_a_struct "指向结构的指针")

    +   [使用 & 运算符](#Using_the_operator "使用 & 运算符")

    +   [使用 new 关键字](#Using_the_new_keyword "使用 new 关键字")

+   [打印结构变量](#Print_a_Struct_Variable "打印结构变量")

    +   [使用 fmt 包](#Using_the_fmt_package "使用 fmt 包")

    +   [以 JSON 形式打印结构](#Printing_the_struct_in_JSON_form "以 JSON 形式打印结构")

+   [结构字段元数据或标签](#Struct_Field_Meta_or_Tags "结构字段元数据或标签")

+   [结构中的匿名字段](#Anonymous_Fields_in_a_Struct "结构中的匿名字段")

+   [嵌套结构](#Nested_Struct "嵌套结构")

+   [匿名嵌套结构字段](#Anonymous_nested_struct_fields "匿名嵌套结构字段")

+   [结构的导出和未导出字段](#Exported_and_UnExported_fields_of_a_struct "结构的导出和未导出字段")

+   [结构相等性](#Struct_Equality "结构相等性")

+   [结构是值类型](#Struct_are_value_types "结构是值类型")

+   [结论](#Conclusion "结论") *  * # **概述**

GO 结构是不同类型数据字段的命名集合。结构作为一个容器，包含不同的异构数据类型，代表一个实体。例如，不同的属性用于表示一个组织中的员工。员工可以具有

+   字符串类型的名称

+   int 类型的年龄

+   time.Time 类型的出生日期

+   int 类型的薪水

..等等。结构可以用来表示一个员工

```
type employee struct {
    name   string
    age    int
    salary int
}
```

golang 中的结构可以与面向对象语言中的类相比较

# **声明一个结构类型**

以下是声明结构的格式

```
type struct_name struct {
    field_name1 field_type1
    field_name2 field_type2
    ...
}
```

在上述格式中，**struct_name**是结构体的名称。它有一个名为**field_name1**的字段，类型为**field_type1**，还有一个名为**field_name2**的字段，类型为**field_type2**。这声明了一个新的命名结构体类型，作为蓝图。类型关键字用于引入新类型。

示例

```
type point struct {
    x float64
    y float64
}
```

上述声明定义了一个名为**point**的新结构体，具有两个字段**x**和**y**。这两个字段都是**float64**类型。一旦声明了新的结构体类型，我们可以从中定义新的具体结构体变量，如我们将在下一节看到的那样。

# **创建一个结构体变量**

声明一个结构体仅声明一个命名的结构体类型。创建一个结构体变量则创建了该结构体的实例，同时也初始化了内存。我们可以创建一个空的结构体变量，而不为任何字段提供值。

```
emp := employee{}
```

在这种情况下，结构体中的所有字段都被初始化为该字段类型的默认零值。

在创建结构体变量时，我们也可以初始化每个结构体字段的值。有两种变体。

+   每个字段在同一行上。

```
emp := employee{name: "Sam", age: 31, salary: 2000}
```

+   每个字段在不同的行上。

```
emp := employee{
   name:   "Sam",
   age:    31,
   salary: 2000,
}
```

只初始化某些字段的值也是可以的。未初始化的字段将获得其类型的默认零值。

```
emp := employee{
   name: "Sam",
   age: 31,
}
```

在上述情况下，由于未初始化，薪水将获得默认值零。

让我们看看一个工作代码，说明上述要点：

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

**输出**

```
Emp1: {name: age:0 salary:0}
Emp2: {name:Sam age:31 salary:2000}
Emp3: {name:Sam age:31 salary:2000}
Emp4: {name:Sam age:31 salary:0}
```

针对上述程序。

+   我们首先声明一个**employee**结构体。

+   emp1的所有字段都被初始化为其类型的默认零值，即姓名为“”，年龄和薪水为0。

+   emp2已在同一行上初始化了所有字段。其字段的值被正确打印。

+   emp3的所有字段在不同的行上初始化。其字段的值被正确打印。

+   emp4的薪水字段被初始化为默认的零值0，而其他两个字段的值则被正确打印。

需要注意的是，在结构体的初始化中，每个新行在花括号内必须以逗号结尾。因此，下面的初始化将引发错误。

```
"salary" : 2000
```

不以逗号结尾。

```
emp := employee{
  name:   "Sam",
  age:    31,
  salary: 2000
}
```

这将是可以的。

```
emp := employee{
  name:   "Sam",
  age:    31,
  salary: 2000}
```

**不带字段名称**

结构体也可以在不指定字段名称的情况下进行初始化。但在这种情况下，必须按顺序提供每个字段的所有值。

```
emp := employee{"Sam", 31, 2000}
```

如果在不使用字段名称时未提供所有值，将引发编译错误。

让我们看看一个程序。

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

**输出**

```
Emp2: {name:Sam age:31 salary:2000}
```

取消注释该行。

```
emp = employee{"Sam", 31}
```

在上面的程序中，它将引发编译器错误。

```
too few values in employee literal
```

# **访问和设置结构体字段**

结构体字段可以通过点运算符访问。获取值的格式如下。

```
n := emp.name
```

同样，也可以为结构体字段分配一个值。

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

**输出**

```
Current name is: Sam
New name is: John
```

# **指向结构体的指针**

创建指向结构体的指针有两种方法。

+   使用&运算符。

+   使用new关键字。

让我们逐一查看上述每种方法。

## **使用&运算符**

**&**运算符可用于获取指向结构体变量的指针。

```
emp := employee{name: "Sam", age: 31, salary: 2000}
empP := &emp
```

结构体指针也可以直接创建

```
empP := &employee{name: "Sam", age: 31, salary: 2000}
```

让我们来看一个程序

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

**输出**

```
Emp: &{name:Sam age:31 salary:2000}
Emp: &{name:John age:30 salary:3000}
```

## **使用new关键字**

使用new()关键字将：

+   创建结构体

+   将所有字段初始化为其类型的零默认值

+   返回指向新创建结构体的指针

这将返回一个指针

```
empP := new(employee)
```

可以使用**%p**格式修饰符打印指针地址

```
fmt.Printf("Emp Pointer: %p\n", empP)
```

解引用运算符‘*’可以用于打印指针所指向的值。

```
fmt.Printf("Emp Value: %+v\n", *empP)
```

它将打印

```
Emp Value: {name: age:0 salary:0}
```

当不使用解引用指针而是使用格式标识符**%+v**时，将在结构体前添加一个&符号，表明这是一个指针。

```
fmt.Printf("Emp Value: %+v\n", empP)
```

它将打印

```
Emp Value: &{name: age:0 salary:0}
```

让我们看看完整程序以说明上述要点

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

**输出**

```
Emp Pointer Address: 0xc000130000
Emp Pointer: &{name: age:0 salary:0}
Emp Value: {name: age:0 salary:0}
```

# **打印一个结构体变量**

有两种方法可以打印所有结构体变量，包括所有的键和值。

+   使用**fmt**包

+   使用**json/encoding**包以JSON形式打印结构体。这也允许以美观的方式打印结构体。

让我们看看两种打印员工结构体实例的方法。

## **使用fmt包**

**fmt.Printf()**函数可以用来打印结构体。可以使用不同的格式标识符以不同方式打印结构体。让我们看看如何使用不同的格式标识符以不同格式打印结构体。

让我们首先创建一个员工实例

```
emp := employee{name: "Sam", age: 31, salary: 2000}
```

+   **%v** – 它将只打印值。字段名称将不会被打印。这是打印结构体的默认方式。例如

```
fmt.Printf("%v", emp)  -  {Sam 31 2000}
```

+   **%+v –** 它将同时打印字段和值。例如

```
fmt.Printf("%+v", emp) - {name:Sam age:31 salary:2000}
```

**fmt.Println()**函数也可以用于打印结构体。由于%v是**fmt.Println()**函数的默认格式，因此输出将与使用%v的**fmt.Printf()**相同。

```
fmt.Println(emp) - {Sam 31 2000}
```

让我们也看看一个工作程序

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

**输出**

```
Emp: {Sam 31 2000}
Emp: {name:Sam age:31 salary:2000}
Emp: main.employee{name:"Sam", age:31, salary:2000}
{Sam 31 2000}
```

## **以JSON格式打印结构体**

第二种方法是以JSON格式打印结构体。**encoding/json**包的**Marshal**和**MarshalIndent**函数可以用来以JSON格式打印结构体。这里是区别

+   **Marshal** – 以下是**Marshal**函数的签名。该函数通过递归遍历值返回**v**的JSON编码。

```
Marshal(v interface{}) ([]byte, error)
```

+   **MarshalIndent**– 以下是**MarshalIndent**函数的签名。它与**Marshal**函数相似，但应用缩进来格式化输出。因此可以用于美观地打印结构体。

```
MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
```

值得注意的是，**Marshal**和**MarshalIndent**函数只能访问结构体的导出字段，这意味着只有大写字段才能被访问并编码为JSON格式。

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

**输出：**

```
Marshal funnction output {"Name":"Sam","Age":31}

MarshalIndent funnction output {
  "Name": "Sam",
  "Age": 31
}
```

薪资字段在输出中没有被打印，因为它以小写字母开头并且没有导出。**Marshal**函数的输出没有格式化，而**MarshalIndent**函数的输出是格式化的。

**golang**还允许通过使用结构体元字段来使JSON编码的结构体键名不同，如下一节所示。

# **结构体字段的元信息或标签**

Go中的结构体也允许为其字段添加元数据。这些元字段可以用于编码解码成不同形式，对结构体字段进行某些形式的验证等。因此，任何元信息都可以与结构体的字段一起存储，并可供任何包或库用于不同目的。

以下是附加元数据的格式。元数据是字符串字面量，即用反引号括起来。

```
type strutName struct{
   fieldName type `key:value key2:value2`
}
```

现在针对我们的用例，我们将为employee结构体添加JSON标签如下。Marshal函数将使用标签中指定的键名。

```
type employee struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    Salary int    `json:"s"`
}
```

让我们看看完整的程序。

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

**输出：**

```
{
  "n": "Sam",
  "a": 31,
  "s": 2000
}
```

输出中的键名与json元标签中指定的相同。

# **结构体中的匿名字段**

结构体可以有匿名字段，也就是说一个字段没有名称。类型将成为字段名称。在下面的例子中，**string**也将是字段名称。

```
type employee struct {
    string
    age    int
    salary int
}
```

匿名字段也可以被访问并赋值。

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

**输出**

```
Current name is: Sam
New name is: John
```

# **嵌套结构体**

结构体可以嵌套另一个结构体。让我们看看嵌套结构体的一个例子。在下面的**employee**结构体中嵌套了**address**结构体。

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

**输出**

```
City: London
Country: UK
```

请注意如何访问嵌套结构体字段。

```
emp.address.city
emp.address.country
```

# **匿名嵌套结构体字段**

嵌套结构体字段也可以是匿名的。在这种情况下，可以直接访问嵌套结构体的字段。所以下面的写法是有效的。

```
emp.city
emp.country
```

还需注意，下面的写法在这种情况下仍然有效。

```
emp.address.city
emp.address.country
```

让我们看看一个程序。

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

**输出**

```
City: London
Country: UK
City: London
Country: UK
```

请注意上面的程序，地址结构体的城市字段可以通过两种方式访问。

```
emp.city
emp.address.city
```

地址结构体的国家字段也是类似的。

# **结构体的导出和未导出字段**

Go没有任何公共、私有或受保护的关键字。控制包外可见性的唯一机制是使用大写和小写格式。

+   **大写标识符**是导出的。大写字母表示这是一个导出标识符，并且在包外可用。

+   **小写标识符**不被导出。小写表示该标识符未被导出，只能在同一包内访问。

所有以大写字母开头的结构体将被导出到其他包。同样，任何以大写字母开头的结构体字段将被导出，否则不会。让我们看看一个示例，展示结构体和结构体字段的导出与未导出。请参见下面的**model.go**和**test.go**。两者都属于**主**包。

+   结构

    +   结构体**Person**是导出的。

    +   结构体**公司**未被导出。

+   结构体的字段

    +   **Person**结构体字段**Name**是导出的。

    +   **Person**结构体字段**age**未被导出，但**Name**是导出的。

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

我们在同一**主**包中写一个文件**test.go**。请看下面。

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

运行该文件时，它能够访问**model.go**中的所有导出和未导出字段，因为两者都位于同一**主**包中。没有编译错误，并且输出如下。

**输出：**

```
&{test 21}
&{}
test
21
```

让我们将上述文件**model.go**移动到一个名为**model**的不同包中。现在注意运行‘go build’时的输出。它会出现编译错误。所有的编译错误都是因为**main**包中的**test.go**无法引用**model**包中**model.go**的未导出字段。

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

**输出：**

```
cannot refer to unexported name model.company
p.age undefined (cannot refer to unexported field or method age)
```

# **结构体相等性**

在考虑结构体相等性之前，首先要了解的是所有结构体字段类型是否可比较。

根据Go规范定义的一些可比较类型有：

+   布尔值

+   数字

+   字符串，

+   指针

+   通道

+   接口类型

+   结构体 – 如果其所有字段类型是可比较的。

+   数组 – 如果数组元素的值类型是可比较的。

根据Go规范，某些类型不可比较，不能用作映射的键：

+   切片

+   映射

+   函数

因此，如果两个结构体的所有字段类型都是可比较的，且所有对应字段的值相等，则它们是相等的。让我们看一个例子。

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

**输出**

```
emp1 annd emp2 are equal
```

如果结构体字段类型不可比较，那么在使用==运算符检查结构体相等性时会出现编译错误。

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

上面的程序会引发编译错误，因为**employee**结构体包含一个字段**departments**，它是**字符串**的一个**切片**。**切片**不是可比较类型，因此导致编译错误。

```
invalid operation: emp1 == emp2 (struct containing []string cannot be compared)
```

# **结构体是值类型**

结构体是Go中的值类型。因此，结构体变量名并不是指向结构体的指针，而是表示整个结构体。当

+   一个结构体变量被赋值给另一个结构体变量。

+   一个结构体变量作为参数传递给一个函数。

让我们通过另一个例子来看上面的要点。

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

**输出**

```
Emp1 Before: {Sam 31 2000}
Emp1 After assignment: {Sam 31 2000}
Emp2: {John 31 2000}
Emp in Test function: {Mike 31 2000}
Emp1 After Test Function Call: {Sam 31 2000}
```

在上面的例子中，

+   我们将**emp1**赋值给**emp2**，然后更改**emp2**的名称为不同的值。之后，当我们打印**emp1**时，发现它没有变化。这是因为当我们将**emp1**赋值给**emp2**时，会创建一个副本，改变**emp2**不会对**emp1**产生任何影响。

+   我们将**emp1**传递给测试函数，然后又在测试函数中更改了它的**name**字段。之后，当我们打印**emp1**时，发现它没有变化。原因是，当**emp1**作为参数传递给测试函数时，创建了**emp1**的副本。

# **结论**

这就是关于Go语言中的结构体的全部内容。在这篇文章中，我们学习了初始化结构体、指向结构体的指针、不同的打印方式、匿名字段等。我希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一篇教程** – [数组](https://golangbyexample.com/understanding-array-golang-complete-guid)

**上一篇教程** – [指针](https://golangbyexample.com/pointer-golang/)

+   [完整指南](https://golangbyexample.com/tag/complete-guide/) *   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
