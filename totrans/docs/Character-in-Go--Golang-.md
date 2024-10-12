<!--yml
category: 未分类
date: 2024-10-13 06:06:46
-->

# Character in Go (Golang)

> 来源：[https://golangbyexample.com/character-in-go/](https://golangbyexample.com/character-in-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code Example](#Code_Example "Code Example")
*   [Caveats](#Caveats "Caveats")*  *# **Overview**

Golang does not have any data type of ‘**char**‘. Therefore

*   **byte** is used to represent the ASCII character. byte is an alias for uint8, hence is of 8 bits or 1 byte and can represent all ASCII characters from 0 to 255

*   **rune** is used to represent all UNICODE characters which include every character that exists. rune is an alias for **int32** and can represent all UNICODE characters. It is 4 bytes in size.

*   A **string** of one length can also be used to represent a character implicitly. The size of one character string will depend upon the encoding of that character. For utf-8 encoding, it will be between 1-4 bytes

To declare either a **byte** or a **rune** we use single quotes. While declaring **byte** we have to specify the type,  If we don’t specify the type, then the default type is meant as a **rune.**

To declare a **string**, we use double quotes or backquotes. Double quotes string honors escape character while back quotes string is a raw literal string and doesn’t honor any kind of escaping.

# **Code Example**

See the program below. It shows

*   A byte representing the character ‘**a**‘

*   A rune representing the pound sign ‘**£**‘

*   A string having one character micro sign **‘µ’**

```
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //If you don't specify type here
    var b byte = 'a'

    fmt.Println("Priting Byte:")
    //Print Size, Type and Character
    fmt.Printf("Size: %d\nType: %s\nCharacter: %c\n", unsafe.Sizeof(b), reflect.TypeOf(b), b)

    r := '£'

    fmt.Println("\nPriting Rune:")
    //Print Size, Type, CodePoint and Character
    fmt.Printf("Size: %d\nType: %s\nUnicode CodePoint: %U\nCharacter: %c\n", unsafe.Sizeof(r), reflect.TypeOf(r), r, r)

    s := "µ" //Micro sign
    fmt.Println("\nPriting String:")
    fmt.Printf("Size: %d\nType: %s\nCharacter: %s\n", unsafe.Sizeof(s), reflect.TypeOf(s), s)
}
```

**Output:**

```
Priting Byte:
Size: 1
Type: uint8
Character: a

Priting Rune:
Size: 4
Type: int32
Unicode CodePoint: U+00A3
Character: £

Priting String:
Size: 16
Type: string
Character: µ
```

# **Caveats**

*   Declaring a  byte with a NON-ASCII character will raise a compiler error as below. I tried with a character having a corresponding code as 285

```
constant 285 overflows byte
```

*   Only a single character can be declared inside a single quote while initializing **byte** or a **rune**. On trying to add two character between single quote, below compiler warning will be generated

```
invalid character literal (more than one character)
```

*   [char](https://golangbyexample.com/tag/char/)*   [character](https://golangbyexample.com/tag/character/)*   [golang](https://golangbyexample.com/tag/golang/)*   [rune](https://golangbyexample.com/tag/rune/)*   [string](https://golangbyexample.com/tag/string/)*