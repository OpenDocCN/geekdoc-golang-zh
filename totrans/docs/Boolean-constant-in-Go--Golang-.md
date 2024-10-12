<!--yml
category: 未分类
date: 2024-10-13 06:28:24
-->

# Boolean constant in Go (Golang)

> 来源：[https://golangbyexample.com/boolean-constant-go/](https://golangbyexample.com/boolean-constant-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")
    *   [Typed boolean constant](#Typed_boolean_constant "Typed boolean constant")
    *   [Untyped named boolean constant](#Untyped_named_boolean_constant "Untyped named boolean constant")
    *   [Untyped unnamed boolean constant](#Untyped_unnamed_boolean_constant "Untyped unnamed boolean constant")*  *# **Overview**

There are two untyped boolean constant **true** and **false**.  Below is the program illustrating a boolean constant. 

To better understand boolean constant in golang it is important to understand typed and untyped constant in go. Please refer to this article for that – [https://golangbyexample.com/typed-untyped-constant-golang](https://golangbyexample.com/typed-untyped-constant-golang)/

Once you have gone through the article you would have understood that constant can be declared in three ways

*   Typed  constant
*   Untyped unnamed  constant
*   Untyped named constant

It is the same case with boolean as well. Let’s see a program to understand it

# **Example**

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

## **Typed boolean constant**

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

## **Untyped named boolean constant**

It is defined as below

```
const bb = true
```

Untyped named string constant can be assigned to variable of type **myBool** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var ww mybool = bb
var xx = bb
```

## **Untyped unnamed boolean constant**

It is like below

```
true
```

Untyped unnamed string constant can be assigned to variable of type **myBool** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var yy mybool = true
var zz = true
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*