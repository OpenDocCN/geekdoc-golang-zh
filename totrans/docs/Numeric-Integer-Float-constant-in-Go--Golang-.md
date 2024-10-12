<!--yml
category: 未分类
date: 2024-10-13 06:28:19
-->

# Numeric/Integer/Float constant in Go (Golang)

> 来源：[https://golangbyexample.com/integer-constant-golang/](https://golangbyexample.com/integer-constant-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")
    *   [Typed integer constant](#Typed_integer_constant "Typed integer constant")
    *   [Untyped named integer constant](#Untyped_named_integer_constant "Untyped named integer constant")
    *   [Untyped unnamed integer constant](#Untyped_unnamed_integer_constant "Untyped unnamed integer constant")
*   [Numeric Expressions](#Numeric_Expressions "Numeric Expressions")*  *# **Overview**

Numeric constant in go are further divided into three types

*   Integer
*   Floats
*   Complex Numbers

To better understand numeric constant in golang it is important to understand typed and untyped constant in go. Please refer to this article for that –[https://golangbyexample.com/typed-untyped-constant-golang](https://golangbyexample.com/typed-untyped-constant-golang)/

Once you have gone through the article you would have understood that constant can be declared in three ways

*   Typed  constant
*   Untyped unnamed  constant
*   Untyped named constant

It is the same case with numeric as well.

A untyped integer constant (both named and unnamed) can be assigned to **int** types, **float** types and **complex** . This is because an int value can be int or float or complex. For eg int value **123** can be

*   A **int** whose value is 123
*   A **float** whose value is 123.0
*   A **complex** whose imaginary part is 0

On the basis of similar logic a untyped **float** constant can be assigned to all **floats** and **complex** types but not **integer** because for eg a float 5.3 cannot be an integer.

On the basis of similar logic a untyped **comple** constant can be assigned to  **complex** types but not **integer** and **float** because for eg a float 5i+3 cannot be an **integer** or a **float**

Let’s see a program to understand it

# **Example**

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

Let’s understand each of them and their behaviour

## **Typed integer constant**

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

## **Untyped named integer constant**

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

## **Untyped unnamed integer constant**

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

# **Numeric Expressions**

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

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*