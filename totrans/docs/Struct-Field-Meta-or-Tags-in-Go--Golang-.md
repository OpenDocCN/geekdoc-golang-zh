<!--yml
category: 未分类
date: 2024-10-13 06:20:10
-->

# Struct Field Meta or Tags in Go (Golang)

> 来源：[https://golangbyexample.com/struct-field-meta-or-tags/](https://golangbyexample.com/struct-field-meta-or-tags/)

A struct in go also allows adding metadata to its fields. These meta fields can be used to encode decode into different forms, doing some forms of validations on struct fields, etc. So basically any meta information can be stored with fields of a struct and can be used by any package or library for different purposes.

Below is the format for attaching a meta-data. Meta-data is a string literal i.e it is enclosed in backquotes

```
type strutName struct{
   fieldName type `key:value key2:value2`
}
```

Let’s see an example of JSON meta or tags. **Marshal** and **MarshalIndent** function of **encoding/json** package can be used to print a struct in JSON format. These functions utilize the JSON meta tags of the struct fields while encoding decoding. While printing these tags will be used as the key names.

Let’s add JSON tags to employee struct as below. **MarshalIndent** function will use the key name specified in the tags

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

The key name in the output is same as specified in the JSON meta tags.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)