<!--yml

分类：未分类

日期：2024-10-13 06:19:29

-->

# 在 Go (Golang)中美观地打印结构体变量

> 来源：[`golangbyexample.com/print-struct-variables-golang/`](https://golangbyexample.com/print-struct-variables-golang/)

**注意：** 如果你对学习 Golang 感兴趣，我们有一个全面的 Golang 教程系列。请查看一下 – [Golang 全面教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)。现在让我们来看当前的教程。以下是目录。

目录

+   概述

+   使用 fmt 包

+   以 JSON 格式打印结构体

# 概述

有两种方法可以打印所有结构体变量，包括所有的键和值。

+   使用**fmt**包

+   使用**json/encoding**包以 JSON 格式打印结构体。这也允许美观地打印结构体。

假设我们有一个员工结构体，如下所示：

```go
type employee struct {
    name   string
    age    int
    salary int
}
```

让我们看看两种打印员工结构体实例的方法。

# **使用 fmt 包**

**fmt.Printf()**函数可以用来打印一个结构体。可以使用不同的格式标识符以不同的方式打印结构体。让我们看看如何使用不同的格式标识符以不同的格式打印结构体。

首先让我们创建一个员工的实例。

```go
emp := employee{name: "Sam", age: 31, salary: 2000}
```

+   **%v** – 仅打印值，字段名不会被打印。这是打印结构体的默认方式。例如：

```go
fmt.Printf("%v", emp)  -  {Sam 31 2000}
```

+   **%+v –** 它将打印字段和值。例如：

```go
fmt.Printf("%+v", emp) - {name:Sam age:31 salary:2000}
```

+   %#v – 它将打印结构体名称，以及字段和值。例如：

```go
fmt.Printf("%#v", emp) - main.employee{name:"Sam", age:31, salary:2000}
```

**fmt.Println()**函数也可以用来打印一个结构体。由于**fmt.Println()**函数的默认值是%v，因此输出将与使用%v 的**fmt.Printf()**相同。

```go
fmt.Println(emp) - {Sam 31 2000}
```

我们也来看一个工作程序。

```go
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

```go
Emp: {Sam 31 2000}
Emp: {name:Sam age:31 salary:2000}
Emp: main.employee{name:"Sam", age:31, salary:2000}
{Sam 31 2000}
```

# **以 JSON 格式打印结构体**

第二种方法是以 JSON 格式打印结构体。**encoding/json**包的**Marshal**和**MarshalIndent**函数可以用来以 JSON 格式打印结构体。这里是区别：

+   **Marshal** – 以下是**Marshal**函数的签名。该函数通过递归遍历值返回**v**的 JSON 编码。

```go
Marshal(v interface{}) ([]byte, error)
```

+   **MarshalIndent**– 以下是**MarshalIndent**函数的签名。它类似于**Marshal**函数，但应用了缩进以格式化输出。因此，可以用来美观地打印一个结构体。

```go
MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
```

需要注意的是，**Marshal**和**MarshalIndent**函数只能访问结构体的导出字段，这意味着只有以大写字母开头的字段才能被访问和编码为 JSON 格式。

```go
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

```go
Marshal funnction output {"Name":"Sam","Age":31}

MarshalIndent funnction output {
  "Name": "Sam",
  "Age": 31
}
```

工资字段未在输出中打印，因为它以小写字母开头且未导出。**Marshal**函数的输出未经过格式化，而**MarshalIndent**函数的输出是格式化的。

**golang** 还允许通过使用结构体元字段使 JSON 编码结构体的键名不同。我们先来理解一下什么是结构体元字段。在 Go 中，结构体也允许为其字段添加元数据。这些元字段可以用于编码解码成不同形式，对结构体字段进行某些形式的验证等。因此，基本上任何元信息都可以与结构体的字段存储，并可以被任何包或库用于不同的目的。

以下是附加元数据的格式。元数据是一个字符串文字，即它被反引号包围。

```go
type strutName struct{
   fieldName type `key:value key2:value2`
}
```

现在针对我们的用例，我们将为员工结构体添加 JSON 标签，如下所示。Marshal 函数将使用标签中指定的键名。

```go
type employee struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    Salary int    `json:"s"`
}
```

让我们看看完整的程序

```go
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

```go
{
  "n": "Sam",
  "a": 31,
  "s": 2000
}
```

输出中的键名与 JSON 元标签中指定的相同。

**打印嵌套结构体**

即使结构体包含另一个结构体，也可以使用上述讨论的方法打印相同内容。

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
)

type address struct {
    City    string `json:"city"`
    Country string `json:"country"`
}

type employee struct {
    Name    string  `json:"name"`
    Age     int     `json:"age"`
    Salary  int     `json:"salary"`
    Address address `json:"address"`
}

func main() {
    address := address{City: "some_city", Country: "some_country"}
    emp := employee{Name: "Sam", Age: 31, Salary: 2000, Address: address}
    //Converting to json
    empJSON, err := json.MarshalIndent(emp, "", "  ")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("MarshalIndent funnction output\n %s\n", string(empJSON))
}
```

**输出**

```go
MarshalIndent function output
 {
  "name": "Sam",
  "age": 31,
  "salary": 2000,
  "address": {
    "city": "some_city",
    "country": "some_country"
  }
}
```

这就是关于打印结构体的全部内容。希望你喜欢这篇文章。请在评论中分享反馈。


