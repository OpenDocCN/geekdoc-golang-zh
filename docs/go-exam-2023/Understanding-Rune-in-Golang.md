<!--yml

类别：未分类

日期：2024-10-13 06:07:42

-->

# 理解 Go 语言中的 Rune

> 来源：[`golangbyexample.com/understanding-rune-in-golang/`](https://golangbyexample.com/understanding-rune-in-golang/)

目录

**概述**

    +   什么是 Unicode

    +   UTF-8

+   何时使用

+   代码：

+   Rune 数组转字符串及反向操作

    +   Rune 数组转字符串

    +   字符串转 Rune 数组*  *# **概述**

在 Go 语言中，**rune** 是 int32 的别名，这意味着它是一个整数值。这个整数值用于表示一个 Unicode 代码点。要理解 rune，你必须知道什么是 Unicode。下面是简短的描述，但你可以参考关于它的著名博客文章 –

[每个软件开发者绝对、一定要知道的关于 Unicode 和字符集的最低要求（没有借口！）](http://www.joelonsoftware.com/articles/Unicode.html)

## **什么是 Unicode**

Unicode 是 ASCII 字符的超集，它为每个存在的字符分配一个唯一的编号。这个唯一的编号称为 Unicode 代码点。例如

+   数字 **0** 表示为 Unicode 点 **U+0030 (十进制值 – 48)**

+   小写字母 **b** 的 Unicode 点表示为 **U+0062 (十进制值 – 98)**

+   英镑符号 **£** 表示为 Unicode 点 **U+00A3 (十进制值 – 163)**

访问 [`en.wikipedia.org/wiki/List_of_Unicode_characters`](https://en.wikipedia.org/wiki/List_of_Unicode_characters) 了解其他字符的 Unicode 点。但 Unicode 不会讨论这些代码点如何在内存中保存。这就是 **utf-8** 的用武之地。

## **UTF-8**

utf-8 将每个 Unicode 点保存为 1、2、3 或 4 字节。ASCII 点使用 1 字节存储。这就是为什么 rune 是 int32 的别名，因为 Unicode 点在 Go 中最多可以为 4 字节，源代码在 Go 中是使用 utf-8 编码的，因此每个字符串也编码为 utf-8。

每个 rune 旨在引用一个 Unicode 点。例如，如果你在类型转换为 rune 数组后打印字符串，它将打印每个字符的 Unicode 点。对于下面的字符串 **“0b£”**，输出将为 – **[U+0030 U+0062 U+00A3]**

```go
fmt.Printf("%U\n", []rune("0b£"))
```

# **何时使用**

+   当你打算在 rune 值中保存 Unicode 代码点时，应使用 rune。当数组中的所有值都意味着一个 Unicode 代码点时，应使用 rune 数组。

+   Rune 也用于表示一个字符。

**声明 Rune**

一个 rune 使用单引号中的字符声明，如下所示，声明一个名为 **‘rPound’** 的变量。

```go
rPound := '£'
```

声明 Rune 后，你还可以执行以下操作

+   **打印类型 –** 输出将为 **int32**

```go
fmt.Printf("Type: %s\n", reflect.TypeOf(rPound))
```

+   **打印 Unicode 代码点 –** 输出将为 **U+00A3**

```go
fmt.Printf("Unicode CodePoint: %U\n", rPound)
```

+   **打印字符 –** 输出将为 **£**

```go
fmt.Printf("Character: %c\n", r)
```

# **代码：**

下面是演示我们讨论的每个要点的代码

```go
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

**输出：**

```go
Size: 4
Type: int32
Unicode CodePoint: U+0061
Character: a
[U+0030 U+0062 U+00A3]
[48 98 163]
```

# **符文数组转换为字符串及其反向操作**

## **符文数组转为字符串**

```go
package main

import "fmt"

func main() {
    runeArray := []rune{'a', 'b', '£'}
    s := string(runeArray)
    fmt.Println(s)
}
```

**输出：**

```go
ab£
```

## **字符串转为符文数组**

```go
package main

import "fmt"

func main() {
    s := "ab£"
    r := []rune(s)
    fmt.Printf("%U\n", r)
}
```

**输出：**

```go
[U+0061 U+0062 U+00A3]
```

+   [Go 语言](https://golangbyexample.com/tag/go/)*   [符文](https://golangbyexample.com/tag/rune/)*
