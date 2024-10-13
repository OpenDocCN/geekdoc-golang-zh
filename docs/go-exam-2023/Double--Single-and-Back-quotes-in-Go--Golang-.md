<!--yml

类别：未分类

日期：2024-10-13 06:06:50

-->

# Go (Golang) 中的双引号、单引号和反引号

> 来源：[https://golangbyexample.com/double-single-back-quotes-go/](https://golangbyexample.com/double-single-back-quotes-go/)

目录

**   [双引号](#Double_quotes "Double quotes")

+   [反引号](#Back_Quotes "Back Quotes")

+   [单引号](#Single_quotes "Single quotes")

+   [示例：](#Example "Example:")*  *# **双引号**

用于定义字符串。用双引号定义的字符串会遵循转义字符。例如，当打印一个包含\n的字符串时，会打印出换行。同样，\t会打印制表符。

# **反引号**

也用于定义字符串。用反引号编码的字符串是原始字面字符串，不会遵循任何转义。

# **单引号**

要声明**字节**或**rune**，我们使用单引号。在声明**字节**时，我们必须指定类型。如果不指定类型，则默认为**rune**。单引号只允许一个字符。在单引号中声明一个包含两个字符的字节或rune时，编译器会产生如下错误。

```go
invalid character literal (more than one character)
```

让我们来看一下上述讨论的所有内容的示例。

+   请注意下面的输出，反引号包围的字符串不遵循\n或\t

+   取消注释下面的行以查看我们上面讨论的编译器错误。

```go
r = 'ab'
```

# **示例：**

```go
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

**输出：**

打印双引号中的字符串：

x 是：tit

for tat

打印反引号中的字符串：

y 是：tit\nfor\ttat

打印字节：

大小：1

类型：uint8

字符：a

打印rune：

大小：4

类型：int32

Unicode 代码点：U+00A3

字符：£

`*   [golang](https://golangbyexample.com/tag/golang/)*   [引号](https://golangbyexample.com/tag/quotes/)*   [单引号](https://golangbyexample.com/tag/single/)`*
