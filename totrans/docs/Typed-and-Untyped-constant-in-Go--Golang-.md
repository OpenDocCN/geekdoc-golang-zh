<!--yml
category: 未分类
date: 2024-10-13 06:27:57
-->

# Typed and Untyped constant in Go (Golang)

> 来源：[https://golangbyexample.com/typed-untyped-constant-golang/](https://golangbyexample.com/typed-untyped-constant-golang/)

Table of Contents

 **   [Typed and Untyped Constants](#Typed_and_Untyped_Constants "Typed and Untyped Constants")
*   [Typed Constant](#Typed_Constant "Typed Constant")
*   [Untyped Constant](#Untyped_Constant "Untyped Constant")*  *# **Typed and Untyped Constants**

In go constant are treated in a different way than any other language. GO has a very strong type system that doesn’t allow implicit conversion between any of the types. Even with the same numeric types no operation is allowed without explicit conversion. For eg you cannot add a **int32** and **int64** value. To add those either **int32** has to be explicitly converted to **int64** or vice versa. However untyped constant have the flexibility of temporary escape from the GO’s type system as we will see in this article

# **Typed Constant**

A const declared specifying the type in the declaration is a typed constant. For example below we are declaring a const of type int32

```
const a int32 = 8
```

This const a can only be assigned to a variable of type int32\. If you assign it to a variable of any other type it will raise an error . For the illustration see below program.

```
package main

func main() {
    const a int32 = 8

    var i1 int32
    var i2 int64

    i1 = a
    i2 = a
}
```

**Output:**

```
cannot use a (type int32) as type int64 in assignment
```

# **Untyped Constant**

An untyped constant is a constant whose type has not been specified. A untyped constant in GO can be either named or unnamed. In both cases it doesn’t have any type associated with it.

Examples of unnamed untyped constant.

```
123        //Default hidden type is int
"circle"   //Default hidden type is string
5.6\.       //Default hidden type is float64
true       //Default hidden type is bool
'a'        //Default hidden type is rune
3+5i       //Default hidden type is complex128
```

Examples of named untyped constant

```
const a = 123        //Default hidden type is int
const b = "circle"   //Default hidden type is string
const c = 5.6       //Default hidden type is float64
const d = true       //Default hidden type is bool
const e = 'a'        //Default hidden type is rune
const f = 3+5i       //Default hidden type is complex128
```

Untyped constant  does have a default hidden type. For example below table illustrates hidden default types for numerics, strings, characters and boolean

Default Hidden type for Constants

| Integers | int |
| Floats | float64 |
| Complex Numbers | complex128 |
| Strings | string |
| Booleans | bool |
| Characters | int32 or rune |

When you print any untyped constant using **fmt.Printf** it will print the default hidden type. See below program and output for both unnamed and named  untyped constant.

```
package main

import "fmt"

func main() {
    //Unanamed untyped constant
    fmt.Printf("Type: %T Value: %v\n", 123, 123)
    fmt.Printf("Type: %T Value: %v\n", "circle", "circle")
    fmt.Printf("Type: %T Value: %v\n", 5.6, 5.6)
    fmt.Printf("Type: %T Value: %v\n", true, true)
    fmt.Printf("Type: %T Value: %v\n", 'a', 'a')
    fmt.Printf("Type: %T Value: %v\n", 3+5i, 3+5i)

    //Named untyped constant
    const a = 123      //Default hidden type is int
    const b = "circle" //Default hidden type is string
    const c = 5.6      //Default hidden type is float64
    const d = true     //Default hidden type is bool
    const e = 'a'      //Default hidden type is rune
    const f = 3 + 5i   //Default hidden type is complex128

    fmt.Println("")
    fmt.Printf("Type: %T Value: %v\n", a, a)
    fmt.Printf("Type: %T Value: %v\n", b, b)
    fmt.Printf("Type: %T Value: %v\n", c, c)
    fmt.Printf("Type: %T Value: %v\n", d, d)
    fmt.Printf("Type: %T Value: %v\n", e, e)
    fmt.Printf("Type: %T Value: %v\n", f, f)
}
```

**Output:**

```
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)

Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
```

The above program prints **int32** instead of rune as rune is an alias for **int32**

The default type of a named or unnamed constant type will become the type of variable they are assigned to. For example, in the below code variable a will get its type from the default type of unnamed constant **123** which is **int**.

```
var a = 123
```

Let’s see a program illustrating above points for all unnamed type constant

```
package main
import "fmt"
func main() {
    //Untyped
    var u = 123      //Default hidden type is int
    var v = "circle" //Default hidden type is string
    var w = 5.6      //Default hidden type is float64
    var x = true     //Default hidden type is bool
    var y = 'a'      //Default hidden type is rune
    var z = 3 + 5i   //Default hidden type is complex128
    fmt.Printf("Type: %T Value: %v\n", u, u)
    fmt.Printf("Type: %T Value: %v\n", v, v)
    fmt.Printf("Type: %T Value: %v\n", w, w)
    fmt.Printf("Type: %T Value: %v\n", x, x)
    fmt.Printf("Type: %T Value: %v\n", y, y)
    fmt.Printf("Type: %T Value: %v\n", z, z)
}
```

**Output:**

```
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
```

Now the question which comes to the mind is what is the use of untyped constant.  The use of untyped constant is that the type of the constant will be decided depending upon the type of variable they are being assigned to.  Sounds confusing? Let’s see with an example.

**Pi** constant value in math package is declared as below.

```
const Pi = 3.14159265358979323846264338327950288419716939937510582097494459
```

Notice that the type is not specified it only has a hidden default type (which is **float64** here).  Let’s see a code

```
package main
import (
    "fmt"
    "math"
)
func main() {
    var f1 float32
    var f2 float64
    f1 = math.Pi
    f2 = math.Pi

    fmt.Printf("Type: %T Value: %v\n", math.Pi, math.Pi)
    fmt.Printf("Type: %T Value: %v\n", f1, f1)
    fmt.Printf("Type: %T Value: %v\n", f2, f2)
}
```

**Output:**

```
Type: float64 Value: 3.141592653589793
Type: float32 Value: 3.1415927
Type: float64 Value: 3.141592653589793
```

Notice above program.

*   Due to untyped nature of **math.Pi** constant it can be assigned to a variable of type **float32** as well as **float64**. This is otherwise not possible in GO after type is fixed.

*   When we print the type of **math.Pi** , it prints the default type which is **float64**

Depending upon use case an untyped constant can be assigned to a low precision type (float32) or a high precision type(float64)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*