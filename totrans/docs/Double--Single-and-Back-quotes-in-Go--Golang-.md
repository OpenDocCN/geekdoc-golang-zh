<!--yml
category: 未分类
date: 2024-10-13 06:06:50
-->

# Double, Single and Back quotes in Go (Golang)

> 来源：[https://golangbyexample.com/double-single-back-quotes-go/](https://golangbyexample.com/double-single-back-quotes-go/)

Table of Contents

 **   [Double quotes](#Double_quotes "Double quotes")
*   [Back Quotes](#Back_Quotes "Back Quotes")
*   [Single quotes](#Single_quotes "Single quotes")
*   [Example:](#Example "Example:")*  *# **Double quotes**

It is used to define a string. A string defined within double quotes will honor escaping characters. For, eg for when printing a string having \n there will be a new line printed. Similarly, \t will have tab printed.

# **Back Quotes**

It is also used to define a string. A string encoded in back quotes is a raw literal string and doesn’t honor any kind of escaping.

# **Single quotes**

To declare either a **byte** or a **rune** we use single quotes. While declaring **byte** we have to specify the type. If we don’t specify the type, then the default type is meant as a **rune.** A single quote will allow only one character. On declaring a byte or rune with two characters within a single quote, the compiler will raise an error as below

```
invalid character literal (more than one character)
```

Let’s see an example of all things discussed above.

*   Notice in below output that string enclosed in back quotes doesn’t honor \n or \t

*   Uncomment the below line to see the compiler error we discussed above.

```
r = 'ab'
```

# **Example:**

```
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //String in double quotes
    x := "tit\nfor\ttat"
    fmt.Println("Priting String in Double Quotes:")
    fmt.Printf("x is: %s\n", x)

   //String in back quotes
    y := `tit\nfor\ttat`
    fmt.Println("\nPriting String in Back Quotes:")
    fmt.Printf("y is: %s\n", y)

    //Declaring a byte with single quotes
    var b byte = 'a'
    fmt.Println("\nPriting Byte:")
    //Print Size, Type and Character
    fmt.Printf("Size: %d\nType: %s\nCharacter: %c\n", unsafe.Sizeof(b), reflect.TypeOf(b), b)

    //Declaring a rune with single quotes
    r := '£'
    fmt.Println("\nPriting Rune:")
    //Print Size, Type, CodePoint and Character
    fmt.Printf("Size: %d\nType: %s\nUnicode CodePoint: %U\nCharacter: %c\n", unsafe.Sizeof(r), reflect.TypeOf(r), r, r)
    //Below will raise a compiler error - invalid character literal (more than one character)
    //r = 'ab'
}
```

**Output:**

Priting String in Double Quotes:
x is: tit
for tat

Priting String in Back Quotes:
y is: tit\nfor\ttat

Priting Byte:
Size: 1
Type: uint8
Character: a

Priting Rune:
Size: 4
Type: int32
Unicode CodePoint: U+00A3
Character: £

`*   [golang](https://golangbyexample.com/tag/golang/)*   [quotes](https://golangbyexample.com/tag/quotes/)*   [single](https://golangbyexample.com/tag/single/)`*