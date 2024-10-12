<!--yml
category: 未分类
date: 2024-10-13 06:05:03
-->

# Find the type of an object in Go (Golang)

> 来源：[https://golangbyexample.com/find-the-type-of-an-object-in-golang/](https://golangbyexample.com/find-the-type-of-an-object-in-golang/)

This article will describe different ways of knowing the type of an object in Go

Table of Contents

 **   [Using Reflect Package](#Using_Reflect_Package "Using Reflect Package")
*   [Using Type Assertion](#Using_Type_Assertion "Using Type Assertion")
*   [Using Switch](#Using_Switch "Using Switch")
*   [Using printf or sprintf](#Using_printf_or_sprintf "Using printf or sprintf")*  *# **Using Reflect Package**

Reflect package provides some useful inspect functions that let us know the type

```
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var test interface{}
    test = "test_string"
    fmt.Println(reflect.TypeOf(test))
}
```

**Output:**

```
string
```

# **Using Type Assertion**

```
package main

import "fmt"

func main() {
    var test interface{}
    test = "test_string"
    val, ok := test.(string)
    if ok {
        fmt.Printf("Test is of type string with value %s\n", val)
    } else {
        fmt.Printf("Unknown Type %T", test)
    }
    test = 2
    val2, ok := test.(int)
    if ok {
        fmt.Printf("Test is of type int with value %d\n", val2)
    } else {
        fmt.Printf("Unknown Type %T", test)
    }
}
```

**Output:**

```
Test is of type string with value test_string
Test is of type int with value 2
```

# **Using Switch**

```
package main

import "fmt"

func main() {
    printType("test_string")
    printType(2)
}

func printType(t interface{}) {
    switch v := t.(type) {
    case string:
        fmt.Println("Type: string")
    case int:
        fmt.Println("Type: int")
    default:
        fmt.Printf("Unknown Type %T", v)
    }
}
```

**Output:**

```
Type: string
Type: int
```

# **Using printf or sprintf**

```
package main

import (
    "fmt"
)

func main() {
    var test interface{}
    test = "test_string"
    //Using Sprintf
    testType := fmt.Sprintf("%T", test)
    fmt.Println(testType)
    //Using printf
    fmt.Printf("%T\n", test)
}
```

**Output:**

```
string
string
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [object](https://golangbyexample.com/tag/object/)*   [type](https://golangbyexample.com/tag/type/)*