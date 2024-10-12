<!--yml
category: 未分类
date: 2024-10-13 06:07:42
-->

# Understanding Rune in Golang

> 来源：[https://golangbyexample.com/understanding-rune-in-golang/](https://golangbyexample.com/understanding-rune-in-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
    *   [What is Unicode](#What_is_Unicode "What is Unicode")
    *   [UTF-8](#UTF-8 "UTF-8")
*   [When to Use](#When_to_Use "When to Use")
*   [Code:](#Code "Code:")
*   [Rune array to string and vice versa](#Rune_array_to_string_and_vice_versa "Rune array to string and vice versa")
    *   [Rune array to string](#Rune_array_to_string "Rune array to string")
    *   [String to Rune Array](#String_to_Rune_Array "String to Rune Array")*  *# **Overview**

**rune** in Go is an alias for int32 meaning it is an integer value. This integer value is meant to represent a Unicode Code Point. To understand rune you have to know what Unicode is. Below is short description but you can refer to the famous blog post about it –

[The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)](http://www.joelonsoftware.com/articles/Unicode.html)

## **What is Unicode**

Unicode is a superset of ASCII characters which assigns a unique number to every character that exists. This unique number is called Unicode Code Point.For eg

*   Digit **0** is represented as Unicode Point **U+0030 (Decimal Value – 48)**

*   Small Case **b** is represented as Unicode Point  **U+0062 (Decimal Value – 98)**

*   A pound symbol **£ **is represented as Unicode Point **U+00A3 (Decimal Value – 163)**

Visit[https://en.wikipedia.org/wiki/List_of_Unicode_characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) to know about Unicode Point of other characters. But Unicode doesn’t talk about how these code points will be saved in memory. This is where **utf-8** comes into picture

## **UTF-8**

utf-8 saves every Unicode Point either using 1, 2, 3 or 4 bytes. ASCII points are stored using 1 byte. That is why rune is an alias for int32 because a Unicode Point can be of max 4 bytes in Go as source code in GO is encoded using utf-8, hence every string is also encoded in utf-8

Every rune is intended to refer to one Unicode Point. For eg if you print a string after typecasting it to a rune array then it will print the Unicode Point for each of character. For for below string **“0b£”** output will be – **[U+0030 U+0062 U+00A3]**

```
fmt.Printf("%U\n", []rune("0b£"))
```

# **When to Use**

*   You should use a rune when you intend to save Unicode Code Point in the rune value. A rune array should be used when all values in the array are meant to be a Unicode Code Point.

*   Rune is also used to represent a character.

**Declare Rune**

A rune is declared using a character between single quotes like below declaring a variable named **‘rPound’**

```
rPound := '£'
```

After declaring Rune you can perform below things as well

*   **Print Type –** Output will be **int32**

```
fmt.Printf("Type: %s\n", reflect.TypeOf(rPound))
```

*   **Print Unicode Code Point –** Output will be **U+00A3**

```
fmt.Printf("Unicode CodePoint: %U\n", rPound)
```

*   **Print Character –** Output will be **£**

```
fmt.Printf("Character: %c\n", r)
```

# **Code:**

Below is the code illustrating each point we discussed

```
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    r := 'a'

    //Print Size
    fmt.Printf("Size: %d\n", unsafe.Sizeof(r))

    //Print Type
    fmt.Printf("Type: %s\n", reflect.TypeOf(r))

    //Print Code Point
    fmt.Printf("Unicode CodePoint: %U\n", r)

    //Print Character
    fmt.Printf("Character: %c\n", r)
    s := "0b£"

    //This will print the Unicode Points
    fmt.Printf("%U\n", []rune(s))

    //This will the decimal value of Unicode Code Point
    fmt.Println([]rune(s))
}
```

**Output:**

```
Size: 4
Type: int32
Unicode CodePoint: U+0061
Character: a
[U+0030 U+0062 U+00A3]
[48 98 163]
```

# **Rune array to string and vice versa**

## **Rune array to string**

```
package main

import "fmt"

func main() {
    runeArray := []rune{'a', 'b', '£'}
    s := string(runeArray)
    fmt.Println(s)
}
```

**Output:**

```
ab£
```

## **String to Rune Array**

```
package main

import "fmt"

func main() {
    s := "ab£"
    r := []rune(s)
    fmt.Printf("%U\n", r)
}
```

**Output:**

```
[U+0061 U+0062 U+00A3]
```

*   [go](https://golangbyexample.com/tag/go/)*   [rune](https://golangbyexample.com/tag/rune/)*