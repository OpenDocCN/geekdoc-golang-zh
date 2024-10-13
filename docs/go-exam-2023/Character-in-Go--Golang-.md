<!--yml

分类：未分类

日期：2024-10-13 06:06:46

-->

# Go语言中的字符（Golang）

> 来源：[https://golangbyexample.com/character-in-go/](https://golangbyexample.com/character-in-go/)

目录

**   [概述](#Overview "Overview")

+   [代码示例](#Code_Example "Code Example")

+   [注意事项](#Caveats "Caveats")*  *# **概述**

Golang没有任何‘**char**’数据类型。因此

+   **byte**用于表示ASCII字符。byte是uint8的别名，因此为8位或1字节，可以表示所有从0到255的ASCII字符

+   **rune**用于表示所有UNICODE字符，包括所有存在的字符。rune是**int32**的别名，能够表示所有UNICODE字符。其大小为4字节。

+   一个长度为一的**string**也可以隐式表示一个字符。一个字符字符串的大小将取决于该字符的编码。对于utf-8编码，它将在1-4字节之间

要声明**byte**或**rune**，我们使用单引号。在声明**byte**时，必须指定类型。如果不指定类型，则默认类型为**rune**。

要声明一个**string**，我们使用双引号或反引号。双引号字符串支持转义字符，而反引号字符串是原始字面字符串，不支持任何转义。

# **代码示例**

请参见下面的程序。它显示

+   表示字符‘**a**‘的byte

+   表示英镑符号‘**£**‘的rune

+   一个字符微符号**‘µ’**的字符串

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

**输出：**

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

# **注意事项**

+   用非ASCII字符声明byte会导致编译器错误，如下所示。我尝试用一个对应代码为285的字符

```
constant 285 overflows byte
```

+   在初始化**byte**或**rune**时，只能在单引号内声明一个字符。当尝试在单引号中添加两个字符时，将生成如下编译器警告

```
invalid character literal (more than one character)
```

+   [char](https://golangbyexample.com/tag/char/)*   [character](https://golangbyexample.com/tag/character/)*   [golang](https://golangbyexample.com/tag/golang/)*   [rune](https://golangbyexample.com/tag/rune/)*   [string](https://golangbyexample.com/tag/string/)*
