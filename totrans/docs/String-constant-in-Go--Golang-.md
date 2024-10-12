<!--yml
category: 未分类
date: 2024-10-13 06:28:06
-->

# String constant in Go (Golang)

> 来源：[https://golangbyexample.com/string-constant-golang/](https://golangbyexample.com/string-constant-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")
    *   [Typed string constant](#Typed_string_constant "Typed string constant")
    *   [Untyped named string constant](#Untyped_named_string_constant "Untyped named string constant")
    *   [Untyped unnamed string constant](#Untyped_unnamed_string_constant "Untyped unnamed string constant")*  *# **Overview**

In go string constant is represented in two ways

*   Any value that is enclosed between double quotes

*   Any value enclosed between back quotes

To better understand string constant in golang it is important to understand typed and untyped constant in go. Please refer to this article for that [https://golangbyexample.com/typed-untyped-constant-golang/](https://golangbyexample.com/typed-untyped-constant-golang/)

Once you have gone through the article you would have understood that constant can be declared in three ways

*   Typed  constant
*   Untyped unnamed  constant
*   Untyped named constant

It is the same case with string as well. Let’s see a program to understand it

# **Example**

Below program shows example of a

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

## **Typed string constant**

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

## **Untyped named string constant**

It is defined as below

```
const bb = "abc"
```

Untyped named string constant can be assigned to variable of type **myString** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var ww myString = bb
var xx = bb
```

## **Untyped unnamed string constant**

It is like below

```
abc
```

Untyped unnamed string constant can be assigned to variable of type **myString** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var yy myString = "abc"
var zz = "abc"
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*