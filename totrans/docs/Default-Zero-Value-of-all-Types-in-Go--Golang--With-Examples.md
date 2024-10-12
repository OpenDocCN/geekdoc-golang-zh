<!--yml
category: 未分类
date: 2024-10-13 06:06:41
-->

# Default Zero Value of all Types in Go (Golang) With Examples

> 来源：[https://golangbyexample.com/go-default-zero-value-all-types/](https://golangbyexample.com/go-default-zero-value-all-types/)

**Default Value Table**

| **Type** | **Default Value** |
| Integer | 0 |
| Float | 0 |
| Complex Number | 0 Real and 0 Imaginary Part |
| Byte | 0 |
| Rune | 0 |
| String | “” |
| Bool | false |
| Array | Every array value to its default |
| Struct | Every field to its default |
| Map | nil |
| Channel | nil |
| Interface | nil |
| Slices | nil |
| Pointer | nil |
| Function | nil |

# **Examples:**

Let’s see an example of each. Use the menu on the side to navigate.

## **Integer**

```
package main

import "fmt"

func main() {
    var a int
    fmt.Print("Default zero value of int: ")
    fmt.Println(a)

    var b uint
    fmt.Print("Default zero value of uint: ")
    fmt.Println(b)
}
```

**Output:**

```
Default zero value of int: 0
Default zero value of uint: 0
```

## **Float**

```
package main
import "fmt"
func main() {
    var a float32
    fmt.Print("Default zero value of float: ")
    fmt.Println(a)
}
```

**Output:**

```
Default zero value of float: 0
```

## **Complex Number**

```
package main
import "fmt"
func main() {
    var a complex64
    fmt.Print("Default zero value of complex: ")
    fmt.Println(a)
}
```

**Output:**

```
Default zero value of complex: (0+0i)
```

## **Byte**

```
package main
import "fmt"
func main() {
    var a byte
    fmt.Print("Default zero value of byte: ")
    fmt.Println(a)
}
```

**Output:**

```
Default zero value of byte: 0
```

## **Rune**

```
package main
import "fmt"
func main() {
    var a rune
    fmt.Print("Default zero value of rune: ")
    fmt.Println(a)
}
```

**Output:**

```
Default zero value of rune: 0
```

## **String**

```
package main
import "fmt"
func main() {
    var a string
    fmt.Print("Default zero value of string: ")
    fmt.Println(a)
}
```

**Output:**

```
Default zero value of string:    //An emtpy string. Hence nothing is printed
```

## **Bool**

```
package main
import "fmt"
func main() {
    var a bool
    fmt.Print("Default zero value of bool: ")
    fmt.Println(a)
}
```

**Output:**

```
Default zero value of bool: false
```

## **Array**

Default value of an array is the default value of its values. For eg in the below code, there is an array of two lengths of type bool. When we print the output is **[false false]**

```
package main
import "fmt"
func main() {
    var a [2]bool
    fmt.Print("Default Zero Value of an array: ")
    fmt.Println(a)
}
```

**Output:**

```
Default Zero Value of a array: [false false]
```

## **Struct**

Default value of a **struct** is the default value of its field. For eg in the below code, there is a struct sample with two field. One is of int type and the other is of the bool type. We create an instance of this struct and when we print it, the output is **{0 false}**

```
package main
import "fmt"
func main() {
    type sample struct {
        a int
        b bool
    }
    a := sample{}
    fmt.Print("Default Zero Value of an struct: ")
    fmt.Println(a)
}
```

**Output:**

```
Default Zero Value of a struct: {0 false} 
```

## **Map**

Default value of a **map** is **nil.** That is why the output of **fmt.Println(a==nil)** is true. When a map is passed to **fmt.Println** , it tries to print values in the map. That is why the output is map[]

```
package main
import "fmt"
func main() {
    var a map[bool]bool
    fmt.Println(a == nil)
    fmt.Print("Printing map: ")
    fmt.Println(a)
}
```

**Output:**

```
true
Printing map: map[]
```

## **Channel**

```
package main
import "fmt"
func main() {
    var a chan int
    fmt.Print("Default zero value of channel: ")
    fmt.Println(a)
}
```

**Output:**

Default zero value of channel: <nil>

## **Interface**

```
package main
import "fmt"
func main() {
    var a chan interface{}
    fmt.Print("Default zero value of interface: ")
    fmt.Println(a)
}
```

**Output:**

Default zero value of interface: < nil>

## **Slice**

Default value of a slice is **nil.** That is why the output of **fmt.Println(a==nil)** is true. When a slice is passed to **fmt.Println** , it tries to print values in the slice. That is why the output is []

```
package main
import "fmt"
func main() {
    var a []int
    fmt.Println(a == nil)
    fmt.Print("Printing slice: ")
    fmt.Println(a)
}
```

**Output:**

```
true
Printing slice: []
```

## **Function**

```
package main
import "fmt"
func main() {
    var a func()
    fmt.Print("Default Zero Value of a func: ")
    fmt.Println(a)
}
```

**Output:**

Default Zero Value of a func: <nil>

## **Pointer**

```
package main
import "fmt"
func main() {
    var a *int
    fmt.Print("Default Zero Value of a pointer: ")
    fmt.Println(a)
}
```

**Output:**

Default Zero Value of a pointer: <nil>

*   [datatype](https://golangbyexample.com/tag/datatype/)*   [default](https://golangbyexample.com/tag/default/)*   [examples](https://golangbyexample.com/tag/examples/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [sidetoc](https://golangbyexample.com/tag/sidetoc/)*   [type](https://golangbyexample.com/tag/type/)*   [zero](https://golangbyexample.com/tag/zero/)