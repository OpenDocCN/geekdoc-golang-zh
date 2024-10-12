<!--yml
category: 未分类
date: 2024-10-13 06:27:39
-->

# Constant in Go (Golang)

> 来源：[https://golangbyexample.com/constant-golang/](https://golangbyexample.com/constant-golang/)

This is the  chapter 9 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [For Loop](https://golangbyexample.com/for-loop-in-golang/)
**Previous Tutorial** –[Functions](https://golangbyexample.com/function-golang-complete-guide/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Important Points](#Important_Points "Important Points")
*   [Typed and Untyped Constants](#Typed_and_Untyped_Constants "Typed and Untyped Constants")
    *   [Typed Constant](#Typed_Constant "Typed Constant")
    *   [Untyped Constant](#Untyped_Constant "Untyped Constant")
*   [Naming Conventions](#Naming_Conventions "Naming Conventions")
*   [Global Constant](#Global_Constant "Global Constant")
*   [Types of Constants](#Types_of_Constants "Types of Constants")
    *   [String Constant](#String_Constant "String Constant")
    *   [Numeric Constant](#Numeric_Constant "Numeric Constant")
    *   [Boolean Constant](#Boolean_Constant "Boolean Constant")
    *   [Character Constant](#Character_Constant "Character Constant")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

A constant is anything that doesn’t change its value. In Go const can be either of type string, numeric, boolean, and characters.

A constant can be declared using the **const** keyword. A important point to be noted is that the value has to be assigned while declaring a constant. It is unlike variables where value can be assigned later.

*   Declaring a **const** along with specifying the type – It starts with a **const** keyword, followed by the name and then the type. Value must also be assigned right away at discussed above

```
const c string = "circle"
```

*   Declaring a const without specifying type – A const declared without type is an untyped constant. We will learn more about typed and untyped constant later on. For now, its important to know that const declared without type has a default hidden type. The constant will be given a type once it is assigned to a variable in any way (direct initialisation, passing to a function etc).

```
const c = "circle"
```

*   Declaring multiple const together.

```
const (
  c = "circle"
  s = "square"
)
```

# **Important Points**

*   Constant Variable Cannot be reassigned after its declaration. For example below code will raise a compilation error

```
package main
func main() {
    const a int = 8
    a = 9
}
```

**Error:**

```
main.go:5:4: cannot assign to a
```

*   const value must be known at compile time. Hence a const value cannot be assigned to a function call which is evaluated at run time. As in below program a is a const and its value should be available at compile time but function **getValue** will only be called at run time hence it raises an error during compilation.

```
package main
const name = "test"
func main() {
    const a = getValue()
}
func getValue() int {
    return 1
}
```

**Error:**

```
const initializer getValue() is not a constant
```

*   A constant declared within an inner having a same name as constant declared in the outer scope will shadow the constant in outer scope.

```
package main
import "fmt"
const a = 123
func main() {
    const a = 456
    fmt.Println(a)
}
```

**Output:**

```
456
```

# **Typed and Untyped Constants**

Now coming to very important topic. In go constant are treated in a different way than any other language. GO has a very strong type system that doesn’t allow implicit conversion between any of the types. Even with the same numeric types no operation is allowed without explicit conversion. For eg you cannot add a **int32** and **int64** value. To add those either **int32** has to be explicitly converted to **int64** or vice versa. However untyped constant have the flexibility of temporary escape from the GO’s type system as we will see in this article

## **Typed Constant**

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

## **Untyped Constant**

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

The default type of a named or unnamed constant type will become type of a variable they are assigned to . For example in below code variable **a** will get its  type from  the default type of unnamed constant **123** which is **int**.

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

**Output**

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

**Output**

```
Type: float64 Value: 3.141592653589793
Type: float32 Value: 3.1415927
Type: float64 Value: 3.141592653589793
```

Notice the above program.

*   Due to the untyped nature of **math.Pi** constant it can be assigned to a variable of type **float32** as well as **float64**. This is otherwise not possible in GO after type is fixed.

*   When we print the type of **math.Pi** , it prints the default type which is **float64**

Depending upon use case an untyped constant can be assigned to a low precision type (float32) or a high precision type(float64)

# **Naming Conventions**

Naming conventions for constant is the same as naming conventions for variables.

*   A constant name can only start with a letter or an underscore. It can be followed by any number of letters, numbers or underscores after that

# **Global Constant**

Like any other variable, a constant will be global within a package if it is declared at the top of a file outside the scope of any function. For example, in the below program name will be a global constant available throughout the **main** package in any function. Do note that the const name will not be available outside the main package. For it to be available outside the main package it has to start with a capital letter.

See the below code. It also shows the example of a local constant within a package.

```
package main

import "fmt"

const name = "test"

func main() {
    const a = 8
    fmt.Println(a)
    testGlobal()
}

func testGlobal() {
    fmt.Println(name)
    //The below line will give compiler error as a is a local constant
    //fmt.Println(a)
```

# **Types of Constants**

Constant can be of four types:

*   Numeric
*   String
*   Character
*   Boolean

## **String Constant**

In go string constant is represented in two ways

*   Any value that is enclosed between double quotes

*   Any value enclosed between back quotes

Below program shows a example of a

*   Typed string constant

*   Untyped unnamed string constant

*   Untyped named string constant

```
package main

import "fmt"

func main() {
	type myString string

	//Typed String constant
	const aa string = "abc"
	var uu = aa
	fmt.Println("Untyped named string constant")
	fmt.Printf("uu: Type: %T Value: %v\n\nn", uu, uu)

	//Below line will raise a compilation error
	//var v myString = aa

	//Untyped named string constant
	const bb = "abc"
	var ww myString = bb
	var xx = bb
	fmt.Println("Untyped named string constant")
	fmt.Printf("ww: Type: %T Value: %v\n", ww, ww)
	fmt.Printf("xx: Type: %T Value: %v\n\n", xx, xx)

	//Untyped unnamed string constant
	var yy myString = "abc"
	var zz = "abc"
	fmt.Println("Untyped unnamed string constant")
	fmt.Printf("yy: Type: %T Value: %v\n", yy, yy)
	fmt.Printf("zz: Type: %T Value: %v\n", zz, zz)

}
```

**Output:**

```
Untyped named string constant
uu: Type: string Value: abc

nUntyped named string constant
ww: Type: main.myString Value: abc
xx: Type: string Value: abc

Untyped unnamed string constant
yy: Type: main.myString Value: abc
zz: Type: string Value: abc
```

In the above program, we created a new type **myString** in code.

```
type myString string
```

Also above program shows the example of

*   Typed string constant
*   Untyped unnamed string constant
*   Untyped named string constant

Let’s understand each of them and their behaviour

**Typed string constant**

It is defined as below

```
const aa string = "abc"
```

Notice in above that below line will cause a compilation error. This is because of typed string constant **aa** is of type **string**. So below line will cause compilation error as it cannot be assigned to variable of type **myString**

```
var v myString = aa
```

But typed string constant can be assigned to a variable created with **var** keyword like below

```
var uu = aa
```

**Untyped named string constant**

It is defined as below

```
const bb = "abc"
```

Untyped named string constant can be assigned to variable of type **myString** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var ww myString = bb
var xx = bb
```

**Untyped unnamed string constant**.

It is like below

```
abc
```

Untyped unnamed string constant can be assigned to variable of type **myString** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var yy myString = "abc"
var zz = "abc"
```

## **Numeric Constant**

Numeric constant are further divided into three types

*   Integer
*   Floats
*   Complex Numbers

A untyped integer constant (both named and unnamed) can be assigned to **int** types, **float** types and **complex** . This is because an int value can be int or float or complex. For eg int value **123** can be

*   A **int** whose value is 123
*   A **float** whose value is 123.0
*   A **complex** whose imaginary part is 0

On the basis of similar logic an untyped **float** constant can be assigned to all **floats** and **complex** types but not **integer** because for eg a float 5.3 cannot be an integer.

On the basis of similar logic, an untyped **complex** constant can be assigned to  **complex** types but not **integer** and **float** because for eg a float 5i+3 cannot be an **integer** or a **float**

See below program illustrating the above point. In the program we have example for

*   Typed integer constant
*   Untyped unnamed integer constant
*   Untyped named integer constant

```
package main

import "fmt"

func main() {
	//Typed int constant
	const aa int = 123
	var uu = aa
	fmt.Println("Typed named integer constant")
	fmt.Printf("uu: Type: %T Value: %v\n\n", uu, uu)

	//Below line will raise a compilation error
	//var v int32 = aa

	//Untyped named int constant
	const bb = 123
	var ww = bb
	var xx int32 = bb
	var yy float64 = bb
	var zz complex128 = bb
	fmt.Println("Untyped named integer constant")
	fmt.Printf("ww: Type: %T Value: %v\n", ww, ww)
	fmt.Printf("xx: Type: %T Value: %v\n", xx, xx)
	fmt.Printf("yy: Type: %T Value: %v\n", yy, yy)
	fmt.Printf("zz: Type: %T Value: %v\n\n", zz, zz)

	//Untyped unnamed int constant
	var ll = 123
	var mm int32 = 123
	var nn float64 = 123
	var oo complex128 = 123
	fmt.Println("Untyped unnamed integer constant")
	fmt.Printf("ll: Type: %T Value: %v\n", ll, ll)
	fmt.Printf("mm: Type: %T Value: %v\n", mm, mm)
	fmt.Printf("nn: Type: %T Value: %v\n", nn, nn)
	fmt.Printf("oo: Type: %T Value: %v\n", oo, oo)
}
```

**Output**

```
Typed named integer constant
uu: Type: int Value: 123

Untyped named integer constant
ww: Type: int Value: 123
xx: Type: int32 Value: 123
yy: Type: float64 Value: 123
zz: Type: complex128 Value: (123+0i)

Untyped unnamed integer constant
ll: Type: int Value: 123
mm: Type: int32 Value: 123
nn: Type: float64 Value: 123
oo: Type: complex128 Value: (123+0i)
```

Now above program shows example of a

*   Typed integer constant
*   Untyped unnamed integer constant
*   Untyped named integer constant

Let’s understand each of them and their behavior

**Typed integer constant**

It is defined as below

```
const aa int = 123
```

Typed integer constant can be assigned to a variable created with **var** keyword like below

```
var uu = aa
```

It will raise compilation error when assigned to another **int** type. So below raises compilation error as **aa** variable is already of type **int**

```
var v int32 = aa
```

**Untyped named integer constant**

It is defined as below

```
const bb = 123
```

Untyped named integer constant can be assigned to any **int** type, any **float** type and any **complex** number type and also to any variable created with **var** keyword. So below works

```
var ww = bb
var xx int32 = bb
var yy float64 = bb
var zz complex128 = bb
```

**Untyped unnamed integer constant**

It is as below

```
123
```

Untyped named integer constant can be assigned to any **int** type, any **float** type and any **complex** number type and also to any variable created with **var** keyword. So below works

```
var ww = 123
var xx int32 = 123
var yy float64 = 123
var zz complex128 = 123
```

**Numeric Expressions**

Due to untyped nature of constant numeric constants, different numeric constant type can be mixed and matched up to form a expression

```
package main
import "fmt"
func main() {
    var p = 5.2 / 3
    fmt.Printf("p: Type: %T Value: %v\n", p, p)
}
```

**Output:**

```
p: Type: float64 Value: 1.7333333333333334
```

## **Boolean Constant**

There are two untyped boolean constant **true** and **false**.  Below is the program illustrating a boolean constant. 

```
package main

import "fmt"

func main() {
	type myBool bool

	//Typed Boolean constant
	const aa bool = true
	var uu = aa
	fmt.Println("Typed named boolean constant")
	fmt.Printf("uu: Type: %T Value: %v\n\n", uu, uu)

	//Below line will raise a compilation error
	//var vv myBool = aa

	//Untyped named boolean constant
	const bb = true

	var ww myBool = bb
	var xx = bb
	fmt.Println("Untyped named boolean constant")
	fmt.Printf("ww: Type: %T Value: %v\n", ww, ww)
	fmt.Printf("xx: Type: %T Value: %v\n\n", xx, xx)

	//Untyped unnamed boolean constant
	var yy myBool = true
	var zz = true
	fmt.Println("Untyped unnamed boolean constant")
	fmt.Printf("yy: Type: %T Value: %v\n", yy, yy)
	fmt.Printf("zz: Type: %T Value: %v\n", zz, zz)
}
```

**Output:**

```
Typed named boolean constant
uu: Type: bool Value: true

Untyped named boolean constant
ww: Type: main.myBool Value: true
xx: Type: bool Value: true

Untyped unnamed boolean constant
yy: Type: main.myBool Value: true
zz: Type: bool Value: true
```

In the above program, we created a new type **myBool**

```
type myBool bool
```

Also above program shows the example of

*   Typed boolean constant
*   Untyped unnamed boolean constant
*   Untyped named boolean constant

Let’s understand each of them and their behaviour

**Typed boolean constant**

It is defined as below

```
const aa bool = true
```

Notice in above that below line will cause a compilation error. This is because variable **aa** is of typed **boolean** constant . So below line will cause compilation error as it cannot be assigned to variable of type **myBool**

```
var v mybool = aa
```

But typed string constant can be assigned to a variable created with **var** keyword like below

```
var uu = aa
```

**Untyped named boolean constant**

It is defined as below

```
const bb = true
```

Untyped named string constant can be assigned to variable of type **myBool** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var ww mybool = bb
var xx = bb
```

**Untyped unnamed boolean constant**

It is like below

```
true
```

Untyped unnamed string constant can be assigned to variable of type **myBool** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var yy mybool = true
var zz = true
```

## **Character Constant**

Below is the program illustrating a character constant.

```
package main

import "fmt"

func main() {
	type myChar int32

	//Typed character constant
	const aa int32 = 'a'
	var uu = aa
	fmt.Println("Untyped unnamed character constant")
	fmt.Printf("uu: Type: %T Value: %v\n\n", uu, uu)

	//Below line will raise a compilation error
	//var vv myBool = aa

	//Untyped named character constant
	const bb = 'a'

	var ww myChar = bb
	var xx = bb
	fmt.Println("Untyped named character constant")
	fmt.Printf("ww: Type: %T Value: %v\n", ww, ww)
	fmt.Printf("xx: Type: %T Value: %v\n\n", xx, xx)

	//Untyped unnamed character constant
	var yy myChar = 'a'
	var zz = 'a'
	fmt.Println("Untyped unnamed character constant")
	fmt.Printf("yy: Type: %T Value: %v\n", yy, yy)
	fmt.Printf("zz: Type: %T Value: %v\n", zz, zz)
}
```

**Output:**

```
Untyped unnamed character constant
uu: Type: int32 Value: 97

Untyped named character constant
ww: Type: main.myChar Value: 97
xx: Type: int32 Value: 97

Untyped unnamed character constant
yy: Type: main.myChar Value: 97
zz: Type: int32 Value: 97
```

In the above program, we created a new type **myChar**

```
type myChar int32
```

Also above program shows the example of

*   Typed character constant
*   Untyped unnamed character constant
*   Untyped named character constant

Let’s understand each of them and their behaviour

**Typed character constant**

It is defined as below

```
const aa int32 = 'a'
```

Notice in above that below line will cause a compilation error. This is because variable **aa** is of type **int32**. So below line will cause compilation error as it cannot be assigned to variable of type **myChar**

```
var v myChar = aa
```

But typed string constant can be assigned to a variable created with **var** keyword like below

```
var uu = aa
```

**Untyped named character constant**

It is defined as below

```
const bb = 'a'
```

Untyped named string constant  can be assigned to variable of type **myChar** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var ww myChar = bb
var xx = bb
```

**Untyped unnamed character constant**.

It is like below

```
'a'
```

Untyped unnamed string constant can be assigned to variable of type **myChar** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var yy myChar = 'a'
var zz = 'a'
```

# **Conclusion**

This is all about const keyword in Golang. Hope you have liked this article. Do share feedback in comments.

**Next Tutorial** – [For Loop](https://golangbyexample.com/for-loop-in-golang/)
**Previous Tutorial** –[Functions](https://golangbyexample.com/function-golang-complete-guide/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*