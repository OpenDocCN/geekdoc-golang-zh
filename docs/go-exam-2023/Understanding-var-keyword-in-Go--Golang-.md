<!--yml

分类：未分类

日期：2024-10-13 06:21:17

-->

# 理解Go中的`var`关键字（Golang）

> 来源：[https://golangbyexample.com/understanding-var-keyword-go/](https://golangbyexample.com/understanding-var-keyword-go/)

**var**关键字是Go语言中的保留关键字，用于声明变量。变量使用`var`关键字声明，但还有其他方式可以声明变量，例如使用`:=`运算符。

以下是使用`var`关键字声明变量的不同方式。

**无初始值的单个变量声明**

以下是单个变量声明的格式，不包含初始值。首先是`var`关键字，其次是变量名称，最后是变量类型。此外，请注意当未提供值时，变量会被初始化为该类型的默认值，这也被称为该类型的零值。在Go中，**int**的默认值或零值是零。

```go
var <variable_name></variable_name> 
```

请看下面的示例，它声明了一个名为**aaa**的类型为**int**的变量。

```go
package main

import "fmt"

func main() {
    var aaa int
    fmt.Println(aaa)
}
```

**输出：** 它将打印**int**的默认值，即零。

```go
0
```

**带初始值的单个变量声明**

以下是带有初始值的单个变量声明格式。与上述相同，唯一不同的是我们在最后给变量赋值。

```go
var <variable_name> <type> = <value></value></type></variable_name>
```

请看下面的示例，它声明了一个名为**aaa**的类型为**int**的变量，并给它赋值为**8**。

```go
package main

import "fmt"

func main() {
    var aaa int = 8
    fmt.Println(aaa)
}
```

**输出：**

```go
8
```

**无初始值的多个变量声明**

以下是多个变量声明的格式，没有赋初值。请注意，只有相同类型的变量可以一起声明。类型在最后出现。

```go
var <name1><name2>etc</name2></name1> 
```

请看下面的示例。

```go
package main

import "fmt"

func main() {
    var aaa, bbb int
    fmt.Println(aaa)
    fmt.Println(bbb)
}
```

**输出：** 它将打印**aaa**和**bbb**的默认或零值，均为零。

```go
0
0
```

**带有初始值的多个变量声明**

以下是带有初始值的多个变量声明的格式。请注意，只有相同类型的变量可以一起声明。类型在最后出现。

```go
var <name1> <name2> etc <type> = <value1> <value2> etc</value2></value1></type></name2></name1>
```

请看下面的代码示例。变量**aaa**和**bbb**在一次声明中分别赋值为8和9。

```go
package main

import "fmt"

func main() {
    var aaa, bbb int = 8, 9
    fmt.Println(aaa)
    fmt.Println(bbb)
}
```

**输出：**

```go
8
9
```

**声明不同类型的变量**

以下是声明多个不同类型变量的格式。此时可以或不可以为变量赋值。未赋值的变量将获得该类型的默认值。在下面的示例中，我们看到三个声明。

```go
package main

import "fmt"

func main() {
    var (
        aaa int
        bbb int    = 8
        ccc string = "a"
    )

    fmt.Println(aaa)
    fmt.Println(bbb)
    fmt.Println(ccc)
}
```

**输出：**

```go
0
8
a
```

**无类型或类型推断的变量声明**

变量也可以在不指定类型的情况下声明。

Go编译器会根据赋给它的值推断类型。因此，如果变量有初始值，则类型可以省略。这也被称为**类型推断**。以下是这种声明的格式。

```go
var <varialbe_name> = value</varialbe_name>
```

以下是基本类型的类型推断表：**int**、**float**、**复数**、**字符串**、**布尔值**、**字符**。这基本上意味着如果值是整数，则推断出的变量类型将是**int**；如果值是浮点数，则推断出的变量类型将是**float64**，依此类推，具体如下表。

| 整数 | int |
| --- | --- |
| 浮点数 | float64 |
| 复数 | complex128 |
| 字符串 | string |
| 布尔值 | bool |
| 字符 | int32 或 rune |

对于其他类型，如**数组**、**指针**、**结构体**等，类型推断将基于值进行。让我们看看上述点的一个工作示例。注意`t`的类型被正确推断为int，因为赋给它的值是123，这是int。同样，`u`的类型也被正确推断为string，因为赋给它的值是一个**字符串**。

还要注意，变量**z**的类型被正确推断为结构体**a**。

```go
package main

import "fmt"

func main() {
    var t = 123      //Type Inferred will be int
    var u = "circle" //Type Inferred will be string
    var v = 5.6      //Type Inferred will be float64
    var w = true     //Type Inferred will be bool
    var x = 'a'      //Type Inferred will be rune
    var y = 3 + 5i   //Type Inferred will be complex128
    var z = &a{name: "test"}

    fmt.Printf("Type: %T Value: %v\n", t, t)
    fmt.Printf("Type: %T Value: %v\n", u, u)
    fmt.Printf("Type: %T Value: %v\n", v, v)
    fmt.Printf("Type: %T Value: %v\n", w, w)
    fmt.Printf("Type: %T Value: %v\n", x, x)
    fmt.Printf("Type: %T Value: %v\n", y, y)
    fmt.Printf("Type: %T Value: %v\n", z, z)
}

type a struct {
    name string
}
```

**输出：**

```go
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
Type: *main.a Value: &{test}
```

+   [Go](https://golangbyexample.com/tag/go/) *   [Golang](https://golangbyexample.com/tag/golang/)
