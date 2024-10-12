<!--yml
category: 未分类
date: 2024-10-13 06:18:02
-->

# Variables in Go (Golang)

> 来源：[https://golangbyexample.com/variables-in-golang-complete-guide/](https://golangbyexample.com/variables-in-golang-complete-guide/)

This is the chapter 6 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [All basic data types](https://golangbyexample.com/all-basic-data-types-golang/)
**Previous Tutorial** – [Packages and Modules – Part 2](https://golangbyexample.com/packages-modules-go-second/)

Now let’s check out the current tutorial. Below is the table of contents for the current tutorial.

Table of Contents

 **   [What is variable](#What_is_variable "What is variable")
*   [Naming Conventions](#Naming_Conventions "Naming Conventions")
*   [Declaring a variable](#Declaring_a_variable "Declaring a variable")
    *   [Single variable declaration without initial value](#Single_variable_declaration_without_initial_value "Single variable declaration without initial value")
    *   [Single variable declaration with initial value](#Single_variable_declaration_with_initial_value "Single variable declaration with initial value")
    *   [Multiple variable declaration without initial value](#Multiple_variable_declaration_without_initial_value "Multiple variable declaration without initial value")
    *   [Multiple variable declaration with initial value](#Multiple_variable_declaration_with_initial_value "Multiple variable declaration with initial value")
    *   [Declare variables of different types](#Declare_variables_of_different_types "Declare variables of different types")
    *   [Variable Declaration with no type or Type Inference](#Variable_Declaration_with_no_type_or_Type_Inference "Variable Declaration with no type or Type Inference")
    *   [Short variable declaration](#Short_variable_declaration "Short variable declaration")
*   [Important Points](#Important_Points "Important Points")
*   [Scope of a Variable (Local and Global Variable)](#Scope_of_a_Variable_Local_and_Global_Variable "Scope of a Variable (Local and Global Variable)")
    *   [Local Variable](#Local_Variable "Local Variable")
    *   [Global Variable](#Global_Variable "Global Variable")
*   [Conclusion](#Conclusion "Conclusion")*  *# **What is variable**

A variable is the name of a memory location. That memory location may store a value of any type. So each variable has a type associated with it which determines the size and range of that variable and also the operations defined on that variable.

# **Naming Conventions**

*   A variable name can only start with a letter or an underscore.

*   It can be followed by any number of letters, numbers or underscores after that

*   Go is case sensitive so uppercase and lowercase letters are treated differently.

*   The variable name cannot be any keyword name in Go

*   There is no limit on the length of the variable name.

*   But it is advisable to have the variable name of optimum length.

# **Declaring a variable**

In GO variables are declared using the **var** keyword but there are other ways of declaring a variable too as we will see later in this tutorial. Let’s explore different ways of declaring a variable

## **Single variable declaration without initial value**

Below is the format of a single variable declaration without initial value being assigned. First is the **var** keyword, second is the **variable name** and third is the **variable type**.  Also note when the value is not provided , then the variable is initialised with the default value of that type which is also known as zero value of that type. In Go default value or zero value of **int** is zero

**var <variable_name> <type>**

See the below example, it declares a var named **aaa** of type **int**

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

## **Single variable declaration with initial value**

Below is the format of a single variable declaration with initial value being assigned. Everything is same as above, only thing additional is that we assign value to the variable at the end

**var <variable_name> <type> = <value>**

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

## **Multiple variable declaration without initial value**

Below is the format of a multiple variable declaration without initial value being assigned. Please note only variables of same type can be declared together. The type comes at the end

**var <name1>, <name2>**,….**<nameN> <type>**

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

## **Multiple variable declaration with initial value**

Below is the format of a multiple variable declaration with initial value being assigned. Please note only variables of same type can be declared together. The type comes at the end

**var <name1>, <name2>, …..,<nameN> <type> = <value1>, <value2>, …..,<valueN>**

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

## **Declare variables of different types**

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

## **Variable Declaration with no type or Type Inference**

Variables can also be declared without specifying the type.

The GO compiler based upon the value assigned to the variable will figure out the type. So if the variable has an initial value, then the type can be omitted.  This is also called **Type Inference**. Below is the format for such declaration

**var <varialbe_name> = <value>**

Below is the type inference table for basic types **int**, **float**, **complex numbers**, **string**, **boolean**, **characters**. It basically means that if the value is an integer the type of the variable inferred will be **int**, if the value is a float then type of the variable inferred will be **float64** and so on based upon below table.

| Integers | int |
| Floats | float64 |
| Complex Numbers | complex128 |
| Strings | string |
| Booleans | bool |
| Characters | int32 or rune |

For other types such as **Array**, **Pointer**, **Structure,** etc, type Inference will happen based on the value. Let’s see a working example of the above point.  Notice that type of t is correctly inferred as int as the value assigned to it is 123 which is int. Similarly type of u is also correctly inferred as **string** as the value assigned to it is a **string**.

Also notice that the type of variable **z** is inferred correctly as a struct **main.sample**

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
    var z = sample{name: "test"}  //Type Inferred will be main.Sample

    fmt.Printf("Type: %T Value: %v\n", t, t)
    fmt.Printf("Type: %T Value: %v\n", u, u)
    fmt.Printf("Type: %T Value: %v\n", v, v)
    fmt.Printf("Type: %T Value: %v\n", w, w)
    fmt.Printf("Type: %T Value: %v\n", x, x)
    fmt.Printf("Type: %T Value: %v\n", y, y)
    fmt.Printf("Type: %T Value: %v\n", z, z)
}

type sample struct {
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
Type: main.sample Value: &{test}
```

## **Short variable declaration**

Go provides another way of declaring variables which is using the **:=** operator. When **:=** operator is used both **var** keyword and type info can be omitted. Below is the format for such declaration

```
 <variable_name>:= <value></value></variable_name>
```

Type inference will happen as explained above. Let’s see a working example

```
package main

import "fmt"

func main() {
    t := 123      //Type Inferred will be int
    u := "circle" //Type Inferred will be string
    v := 5.6      //Type Inferred will be float64
    w := true     //Type Inferred will be bool
    x := 'a'      //Type Inferred will be rune
    y := 3 + 5i   //Type Inferred will be complex128
    z := sample{name: "test"}  //Type Inferred will be main.Sample

    fmt.Printf("Type: %T Value: %v\n", t, t)
    fmt.Printf("Type: %T Value: %v\n", u, u)
    fmt.Printf("Type: %T Value: %v\n", v, v)
    fmt.Printf("Type: %T Value: %v\n", w, w)
    fmt.Printf("Type: %T Value: %v\n", x, x)
    fmt.Printf("Type: %T Value: %v\n", y, y)
    fmt.Printf("Type: %T Value: %v\n", z, z)
}

type sample struct {
    name string
}
```

**Output**

```
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
Type: main.sample Value: &{test}
```

Some points to be noted about the := operator

*   := operator is only available within a function. It is not allowed outside the function.
*   A variable once declared using := cannot be redeclared using the := operator. So below statement will raise a compiler error  “**no new variables in the left side of :=”** .

```
a := 8
a := 16
```

*   := operator can also be used to declare multiple variables in a single line. See below example

```
a,b := 1, 2
```

*   In case of multiple declaration, := can also be used again for a particular variable if atleast one of the variables on left hand side is new. See below example. Notice that b is again declared using := This is only possible if atleast one of the variable is new which is variable **c** here. In this case it acts as a assignment for variable **b**

```
package main

import "fmt"

func main() {
    a, b := 1, 2
    b, c := 3, 4
    fmt.Println(a, b, c)
}
```

**Output:**

```
1, 3, 4
```

# **Important Points**

*   A unused variable will be reported as a compiler error. GO compiler doesn’t allow any unused variable. This is an optimization in GO. Same is applicable for constant too as we will see later. For eg below program will raise a compiler error

```
a declared but not used
```

```
package main

func main() {
    var a = 1
}
```

*   A variable declared within an inner scope having the same name as variable declared in the outer scope will shadow the variable in the outer scope.

```
package main

import "fmt"

var a = 123

func main() {
    var a = 456
    fmt.Println(a)
}
```

**Output:**

```
456
```

*   Variable Expression – While the declaration variable can also be assigned an expression or a function call. See the below example.
    *   Variable **a** is declared with expression 5 +3
    *   Variable **b** is declared with a function call  math.Max(4, 5), whose result will be assigned to b at run time.

```
package main
import (
    "fmt"
    "math"
)
func main() {
    a := 5 + 3
    b := math.Max(4, 5)
    fmt.Println(a)
    fmt.Println(b)
}
```

**Output:**

```
8
5
```

*   A variable once intialized with a particular type, cannot be assigned a value of different type later. This is applicable for short hand declaration is well. See below example

```
package main

func main() {
    var aaa int = 1
    aaa = "atest"

    bbb := 1
    bbb = "btest"
}
```

**Output:**

```
cannot use "atest" (type untyped string) as type int in assignment
cannot use "btest" (type untyped string) as type int in assignment
```

Variable **aaa** has been assigned a type of **int**, hence compiler raises error on assigning a value of type string to it. For variable **bbb**, the type inferred is **int,** hence it also raises a compiler error on assigning a value of type string to it.

# **Scope of a Variable (Local and Global Variable)**

A variable declaration can be done at the package level or a function level or a block level. Scope of a variable defines where that variable is accessible and also the life of the variable. Golang variables can be divided into two categories based on scope

*   Local Variable

*   Global Variable

## **Local Variable**

*   Local variables are variables which are defined within a block or a function level

*   An example of the block is a for loop or a range loop etc.

*   These variables are only be accessed from within their block or function

*   These variables only live till the end of the block or a function in which they are declared. After that, they are Garbage Collected.

See below example

*   i is not available after the for loop

*   Similarly aaa is not available outside the function in which it is declared.

Hence below program will raise compiler error

```
undefined: i
undefined: aaa
```

**Code:**

```
package main

import "fmt"

func main() {
    var aaa = "test"
    fmt.Println(aaa)
    for i := 0; i < 3; i++ {
        fmt.Println(i)
    }
    fmt.Println(i)
}

func testLocal() {
    fmt.Println(aaa)
}
```

## **Global Variable**

*   A variable will be global within a package if it is declared at the top of a file outside the scope of any function or block.

*   If this variable name starts with a lowercase letter then it can be accessed from within the the package which contains this variable definition.

*   If the variable name stats with a uppercase letter then it can be accessed from outside different package other than which it is declared.

*   Global variable are available throughout the lifetime of a program

For example in below program variable **aaa** will be a global variable available throughout **main** package. It will be available in any function inside **main** package. Do note that variable name will not be available outside **main** package as its name starts with a lowercase letter.

```
package main

import "fmt"

var aaa = "test"

func main() {
    testGlobal()
}

func testGlobal() {
    fmt.Println(aaa)
}
```

**Output:**

```
test
```

# **Conclusion**

This is all about variables  in golang. Hope you have liked this article. Please share feedback or improvements or mistakes in comments.

****Next Tutorial** – [All basic data types](https://golangbyexample.com/all-basic-data-types-golang/)
**Previous Tutorial** – [Packages and Modules – Part 2](https://golangbyexample.com/packages-modules-go-second/)**

*   [golang](https://golangbyexample.com/tag/golang/)*   [understanding variables in go](https://golangbyexample.com/tag/understanding-variables-in-go/)*   [using variables in go](https://golangbyexample.com/tag/using-variables-in-go/)*