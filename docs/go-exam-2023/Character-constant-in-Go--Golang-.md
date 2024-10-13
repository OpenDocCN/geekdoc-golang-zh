<!--yml

分类：未分类

日期：2024-10-13 06:28:28

-->

# Go (Golang) 中的字符常量

> 来源：[https://golangbyexample.com/character-constant-golang/](https://golangbyexample.com/character-constant-golang/)

目录

**   [概述](#Overview "概述")

+   [示例](#Example "示例")

    +   [类型字符常量](#Typed_character_constant "类型字符常量")

    +   [未类型命名字符常量](#Untyped_named_character_constant "未类型命名字符常量")

    +   [未类型未命名字符常量](#Untyped_unnamed_character_constant "未类型未命名字符常量")*  *# **概述**

在Go中，字符常量表示为单引号之间的值。请参考这篇文章以更好地理解Golang中的字符。

同样，为了更好地理解Golang中的字符常量，理解Go中的类型和未类型常量是很重要的。请参考这篇文章了解更多 – [https://golangbyexample.com/typed-untyped-constant-golang](https://golangbyexample.com/typed-untyped-constant-golang)/

一旦你阅读完这篇文章，你将会明白常量可以用三种方式声明

+   类型常量

+   未类型未命名常量

+   未类型命名常量

字符的情况也是如此。让我们看一个程序来理解它

# **示例**

下面是一个说明字符常量的程序。

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

**输出：**

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

在上述程序中，我们创建了一个新类型**myChar**

```
type myChar int32
```

上述程序还展示了

+   类型字符常量

+   未类型未命名字符常量

+   未类型命名字符常量

让我们理解它们的每一个及其行为

## **类型字符常量**

定义如下

```
const aa int32 = 'a'
```

请注意，上述代码中的以下行将导致编译错误。这是因为变量**aa**的类型为**int32**。因此，下面的行将导致编译错误，因为它无法赋值给类型为**myChar**的变量。

```
var v myChar = aa
```

但是类型字符串常量可以赋值给使用**var**关键字创建的变量，如下所示

```
var uu = aa
```

## **未类型命名字符常量**

定义如下

```
const bb = 'a'
```

未类型命名字符串常量可以赋值给类型为**myChar**的变量以及使用**var**关键字创建的变量，因为它是未类型的，所以常量的类型将根据赋值给它的变量类型决定。

```
var ww myChar = bb
var xx = bb
```

## **未类型未命名字符常量**

它如下所示

```
'a'
```

未类型未命名字符串常量可以赋值给类型为**myChar**的变量以及使用**var**关键字创建的变量，因为它是未类型的，所以常量的类型将根据赋值给它的变量类型决定。

```
var yy myChar = 'a'
var zz = 'a'
```

+   [Go](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*
