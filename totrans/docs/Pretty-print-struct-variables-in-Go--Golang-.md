<!--yml
category: 未分类
date: 2024-10-13 06:19:29
-->

# Pretty print struct variables in Go (Golang)

> 来源：[https://golangbyexample.com/print-struct-variables-golang/](https://golangbyexample.com/print-struct-variables-golang/)

**Note:** If you are interested in learning Golang, then for that we have a golang comprehensive tutorial series. Do check it out – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/). Now let’s see the current tutorial. Below is the table of contents.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Using the fmt package](#Using_the_fmt_package "Using the fmt package")
*   [Printing the struct in JSON form](#Printing_the_struct_in_JSON_form "Printing the struct in JSON form")*  *# Overview

Thereare two ways to print all struct variables including all its key and values.

*   Using the **fmt** package

*   Printing the struct in JSON form using the **json/encoding** package. This also allows pretty print of a struct as well.

Let’s say we have an employee struct as below:

```
type employee struct {
    name   string
    age    int
    salary int
}
```

Let’s see the two ways in which we can print the instance of the employee struct.

# **Using the fmt package**

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

*   %#v – It will print the struct name, also both field and value. Eg

```
fmt.Printf("%#v", emp) - main.employee{name:"Sam", age:31, salary:2000}
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

# **Printing the struct in JSON form**

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

**golang** also allows the JSON encoded struct key name to be different by the use of struct meta fields. Let’s first understand what is struct meta fields. A struct in go also allows adding metadata to its fields. These meta fields can be used to encode decode into different forms, doing some forms of validations on struct fields, etc. So basically any meta information can be stored with fields of a struct and can be used by any package or library for different purposes.

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

The key name in the output is the same as specified in the JSON meta tags.

**Printing nested struct**

Even if the struct contains another struct , same method as discussed above can be used to print the same

```
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

**Output**

```
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

This is all about printing a struct. I hope you have liked this article. Please share the feedback in the comments.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [pretty](https://golangbyexample.com/tag/pretty/)*