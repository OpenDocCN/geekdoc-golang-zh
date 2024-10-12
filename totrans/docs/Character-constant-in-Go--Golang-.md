<!--yml
category: 未分类
date: 2024-10-13 06:28:28
-->

# Character constant in Go (Golang)

> 来源：[https://golangbyexample.com/character-constant-golang/](https://golangbyexample.com/character-constant-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")
    *   [Typed character constant](#Typed_character_constant "Typed character constant")
    *   [Untyped named character constant](#Untyped_named_character_constant "Untyped named character constant")
    *   [Untyped unnamed character constant](#Untyped_unnamed_character_constant "Untyped unnamed character constant")*  *# **Overview**

In go character constant is represented as a value between single quotes. Refer this article to understand character in golang better in general

Also to better understand character constant in golang it is important to understand typed and untyped constant in go. Please refer to this article for that – [https://golangbyexample.com/typed-untyped-constant-golang](https://golangbyexample.com/typed-untyped-constant-golang)/

Once you have gone through the article you would have understood that constant can be declared in three ways

*   Typed  constant
*   Untyped unnamed  constant
*   Untyped named constant

It is the same case with character as well. Let’s see a program to understand it

# **Example**

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

In the above program, we created a new type  **myChar**

```
type myChar int32
```

Also above program shows the example of

*   Typed character constant
*   Untyped unnamed character constant
*   Untyped named character constant

Let’s understand each of them and their behaviour

## **Typed character constant**

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

## **Untyped named character constant**

It is defined as below

```
const bb = 'a'
```

Untyped named string constant  can be assigned to variable of type **myChar** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var ww myChar = bb
var xx = bb
```

## **Untyped unnamed character constant**

It is like below

```
'a'
```

Untyped unnamed string constant can be assigned to variable of type **myChar** as well variable created with **var** keyword because it is untyped so type of the constant will be decided depending upon the type of variable they are being assigned to.

```
var yy myChar = 'a'
var zz = 'a'
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*