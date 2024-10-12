<!--yml
category: 未分类
date: 2024-10-13 06:20:15
-->

# Method on a non-struct type in Go (Golang)

> 来源：[https://golangbyexample.com/method-non-struct-type-golang/](https://golangbyexample.com/method-non-struct-type-golang/)

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [method](https://golangbyexample.com/tag/method/)