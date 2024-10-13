<!--yml

类别：未分类

日期：2024-10-13 06:05:03

-->

# 在 Go (Golang) 中查找对象的类型

> 来源：[`golangbyexample.com/find-the-type-of-an-object-in-golang/`](https://golangbyexample.com/find-the-type-of-an-object-in-golang/)

本文将描述在 Go 中了解对象类型的不同方法

目录

**   使用反射包

+   使用类型断言

+   使用 Switch

+   使用 printf 或 sprintf

# **使用反射包**

反射包提供了一些有用的检查函数，可以让我们了解类型

```go
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

**输出：**

```go
string
```

# **使用类型断言**

```go
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

**输出：**

```go
Test is of type string with value test_string
Test is of type int with value 2
```

# **使用 Switch**

```go
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

**输出：**

```go
Type: string
Type: int
```

# **使用 printf 或 sprintf**

```go
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

**输出：**

```go
string
string
```


