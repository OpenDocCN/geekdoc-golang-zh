<!--yml
category: 未分类
date: 2024-10-13 06:21:17
-->

# Understanding var keyword in Go (Golang)

> 来源：[https://golangbyexample.com/understanding-var-keyword-go/](https://golangbyexample.com/understanding-var-keyword-go/)

**var** keyword is a reserved keyword in golang which is used to declare variables in go .variables are declared using the **var** keyword but there are other ways of declaring a variable too such as using **:=** operator

Below are different ways of declaring a variable using the var keyword.

**Single variable declaration without an initial value**

Below is the format of a single variable declaration without the initial value being assigned. First is the var keyword, second is the variable name and third is the variable type.  Also note when the value is not provided, then the variable is initialized with the default value of that type which is also known as zero value of that type. In Go default value or zero value of **int** is zero

```
var <variable_name></variable_name> 
```

See below example, it declares a var named **aaa** of type **int**

```
package main

import "fmt"

func main() {
    var aaa int
    fmt.Println(aaa)
}
```

**Output:** It will print the default value of int which is zero

```
0
```

**Single variable declaration with initial value**

Below is the format of a single variable declaration with initial value being assigned. Everything is same as above, only thing additional is that we assign value to the variable at the end

```
var <variable_name> <type> = <value></value></type></variable_name>
```

See below example, it declares a var named **aaa** of type **int** and gives a value **8** to it

```
package main

import "fmt"

func main() {
    var aaa int = 8
    fmt.Println(aaa)
}
```

**Output:**

```
8
```

**Multiple variable declaration without initial value**

Below is the format of a multiple variable declaration without initial value being assigned. Please note only variables of same type can be declared together. The type comes at the end

```
var <name1><name2>etc</name2></name1> 
```

See below example

```
package main

import "fmt"

func main() {
    var aaa, bbb int
    fmt.Println(aaa)
    fmt.Println(bbb)
}
```

**Output:** It will print the default or zero value of **int** which is zero for both **aaa** and **bbb**

```
0
0
```

**Multiple variable declaration with initial value**

Below is the format of a multiple variable declaration with initial value being assigned. Please note only variables of same type can be declared together. The type comes at the end

```
var <name1> <name2> etc <type> = <value1> <value2> etc</value2></value1></type></name2></name1>
```

See below code example. Variable **aaa** and **bbb** are given a value of 8 and 9 respectively in a single declaration

```
package main

import "fmt"

func main() {
    var aaa, bbb int = 8, 9
    fmt.Println(aaa)
    fmt.Println(bbb)
}
```

**Output:**

```
8
9
```

**Declare variables of different types**

Below is the format for declaring multiple variables with different types. Value can or cannot be assigned to the variable at that time. Variables for which value is not assigned will get default value of that type. In below example we see three declarations.

```
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

**Output:**

```
0
8
a
```

**Variable Declaration with no type or Type Inference**

Variables can also be declared without specifying the type.

The GO compiler based upon the value assigned to it will figure out the type. So if the variable has an initial value, then type can be omitted.  This is also called **Type Inference**. Below is the format for such declaration

```
var <varialbe_name> = value</varialbe_name>
```

Below is the type inference table for basic types **int**, **float**, **complex numbers**, **string**, **boolean**, **characters**. It basically means that if the value is an integer the type of the variable inferred will be **int**, if the value is a float then type of the variable inferred will be **float64** and so on based upon below table.

| Integers | int |
| Floats | float64 |
| Complex Numbers | complex128 |
| Strings | string |
| Booleans | bool |
| Characters | int32 or rune |

For other types such as **Array**, **Pointer**, **Structure,** etc, type Inference will happen based on the value. Let’s see a working example of the above point.  Notice that type of t is correctly inferred as int as the value assigned to it is 123 which is int. Similarly type of u is also correctly inferred as string as the value assigned to it is a **string**.

Also notice that the type of variable **z** is inferred correctly as a struct **a**

```
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

**Output:**

```
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
Type: *main.a Value: &{test}
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)