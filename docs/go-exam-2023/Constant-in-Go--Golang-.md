<!--yml

类别：未分类

日期：2024-10-13 06:27:39

-->

# Go（Golang）中的常量

> 来源：[https://golangbyexample.com/constant-golang/](https://golangbyexample.com/constant-golang/)

这是golang综合教程系列的第9章。有关该系列其他章节，请参考此链接 – [Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [for循环](https://golangbyexample.com/for-loop-in-golang/)

**上一个教程** – [函数](https://golangbyexample.com/function-golang-complete-guide/)

现在让我们查看当前的教程。以下是当前教程的目录。

目录

**[概述](#Overview "Overview")**

+   [重要点](#Important_Points "Important Points")

+   [类型和未类型常量](#Typed_and_Untyped_Constants "Typed and Untyped Constants")

    +   [类型常量](#Typed_Constant "Typed Constant")

    +   [未类型常量](#Untyped_Constant "Untyped Constant")

+   [命名约定](#Naming_Conventions "Naming Conventions")

+   [全局常量](#Global_Constant "Global Constant")

+   [常量类型](#Types_of_Constants "Types of Constants")

    +   [字符串常量](#String_Constant "String Constant")

    +   [数字常量](#Numeric_Constant "Numeric Constant")

    +   [布尔常量](#Boolean_Constant "Boolean Constant")

    +   [字符常量](#Character_Constant "Character Constant")

+   [结论](#Conclusion "Conclusion") * * # **概述**

常量是任何不会改变其值的东西。在Go中，const可以是字符串、数字、布尔和字符类型。

常量可以使用**const**关键字声明。需要注意的一个重要点是，在声明常量时必须赋值。这与变量不同，变量的值可以稍后赋值。

+   声明一个**const**并指定类型 – 它以**const**关键字开头，后面是名称，然后是类型。值必须立即赋值，如上所述。

```
const c string = "circle"
```

+   声明一个不指定类型的const – 一个没有指定类型的const是一个未类型化常量。我们稍后将深入了解类型常量和未类型常量。现在，重要的是要知道未指定类型的const具有默认的隐藏类型。常量一旦以任何方式（直接初始化、传递给函数等）分配给变量，就会被赋予类型。

```
const c = "circle"
```

+   一次声明多个const。

```
const (
  c = "circle"
  s = "square"
)
```

# **重要点**

+   常量变量在声明后不能重新赋值。例如，下面的代码将引发编译错误。

```
package main
func main() {
    const a int = 8
    a = 9
}
```

**错误：**

```
main.go:5:4: cannot assign to a
```

+   const值必须在编译时已知。因此，const值不能赋值给运行时评估的函数调用。在下面的程序中，a是一个const，其值应该在编译时可用，但函数**getValue**将在运行时调用，因此在编译时会引发错误。

```
package main
const name = "test"
func main() {
    const a = getValue()
}
func getValue() int {
    return 1
}
```

**错误：**

```
const initializer getValue() is not a constant
```

+   在内部声明的常量如果与外部作用域中声明的常量同名，则会遮蔽外部作用域中的常量。

```
package main
import "fmt"
const a = 123
func main() {
    const a = 456
    fmt.Println(a)
}
```

**输出：**

```
456
```

# **类型化与未类型化常量**

现在谈谈一个非常重要的话题。在 GO 中，常量的处理方式与任何其他语言不同。GO 具有非常强的类型系统，不允许不同类型之间的隐式转换。即使是相同的数字类型，没有显式转换也不允许进行操作。例如，你不能将 **int32** 和 **int64** 值相加。要相加，必须将 **int32** 显式转换为 **int64** 或反之。然而，未类型化常量具有暂时逃避 GO 类型系统的灵活性，正如我们将在本文中看到的。

## **类型化常量**

声明时指定类型的常量是类型化常量。例如，下面我们声明一个类型为 int32 的常量。

```
const a int32 = 8
```

这个常量 a 只能赋值给类型为 int32 的变量。如果你将其赋值给任何其他类型的变量，将会引发错误。请参见下面的程序进行说明。

```
package main

func main() {
    const a int32 = 8

    var i1 int32
    var i2 int64

    i1 = a
    i2 = a
}
```

**输出：**

```
cannot use a (type int32) as type int64 in assignment
```

## **未类型化常量**

未类型化常量是类型未被指定的常量。GO 中的未类型化常量可以是命名的或未命名的。在这两种情况下，它都没有任何相关的类型。

未命名未类型化常量的示例。

```
123        //Default hidden type is int
"circle"   //Default hidden type is string
5.6\.       //Default hidden type is float64
true       //Default hidden type is bool
'a'        //Default hidden type is rune
3+5i       //Default hidden type is complex128
```

命名未类型化常量的示例。

```
const a = 123        //Default hidden type is int
const b = "circle"   //Default hidden type is string
const c = 5.6       //Default hidden type is float64
const d = true       //Default hidden type is bool
const e = 'a'        //Default hidden type is rune
const f = 3+5i       //Default hidden type is complex128
```

未类型化常量确实具有默认的隐藏类型。例如，下面的表格展示了数字、字符串、字符和布尔值的隐藏默认类型。

常量的默认隐藏类型

| 整数 | int |
| --- | --- |
| 浮点数 | float64 |
| 复数 | complex128 |
| 字符串 | string |
| 布尔值 | bool |
| 字符 | int32 或 rune |

当你使用 **fmt.Printf** 打印任何未类型化的常量时，它将打印默认的隐藏类型。请查看下面的程序和未命名与命名未类型化常量的输出。

```
package main

import "fmt"

func main() {
    //Unanamed untyped constant
    fmt.Printf("Type: %T Value: %v\n", 123, 123)
    fmt.Printf("Type: %T Value: %v\n", "circle", "circle")
    fmt.Printf("Type: %T Value: %v\n", 5.6, 5.6)
    fmt.Printf("Type: %T Value: %v\n", true, true)
    fmt.Printf("Type: %T Value: %v\n", 'a', 'a')
    fmt.Printf("Type: %T Value: %v\n", 3+5i, 3+5i)

    //Named untyped constant
    const a = 123      //Default hidden type is int
    const b = "circle" //Default hidden type is string
    const c = 5.6      //Default hidden type is float64
    const d = true     //Default hidden type is bool
    const e = 'a'      //Default hidden type is rune
    const f = 3 + 5i   //Default hidden type is complex128

    fmt.Println("")
    fmt.Printf("Type: %T Value: %v\n", a, a)
    fmt.Printf("Type: %T Value: %v\n", b, b)
    fmt.Printf("Type: %T Value: %v\n", c, c)
    fmt.Printf("Type: %T Value: %v\n", d, d)
    fmt.Printf("Type: %T Value: %v\n", e, e)
    fmt.Printf("Type: %T Value: %v\n", f, f)
}
```

**输出：**

```
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)

Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
```

上述程序打印 **int32** 而不是字符，因为字符是 **int32** 的别名。

命名或未命名常量的默认类型将成为它们赋值的变量的类型。例如，在下面的代码中，变量 **a** 将从未命名常量 **123** 的默认类型 **int** 中获得类型。

```
var a = 123
```

让我们看一个程序，说明上述所有未命名类型常量的要点。

```
package main
import "fmt"
func main() {
    //Untyped
    var u = 123      //Default hidden type is int
    var v = "circle" //Default hidden type is string
    var w = 5.6      //Default hidden type is float64
    var x = true     //Default hidden type is bool
    var y = 'a'      //Default hidden type is rune
    var z = 3 + 5i   //Default hidden type is complex128
    fmt.Printf("Type: %T Value: %v\n", u, u)
    fmt.Printf("Type: %T Value: %v\n", v, v)
    fmt.Printf("Type: %T Value: %v\n", w, w)
    fmt.Printf("Type: %T Value: %v\n", x, x)
    fmt.Printf("Type: %T Value: %v\n", y, y)
    fmt.Printf("Type: %T Value: %v\n", z, z)
}
```

**输出**

```
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
```

现在我们想到的问题是未类型化常量有什么用。未类型化常量的作用在于，常量的类型将根据赋值的变量类型来决定。听起来令人困惑？让我们通过一个例子来看。

数学包中的 **Pi** 常量值声明如下。

```
const Pi = 3.14159265358979323846264338327950288419716939937510582097494459
```

请注意，类型未被指定，只具有一个隐藏的默认类型（这里是 **float64**）。让我们看一段代码。

```
package main
import (
    "fmt"
    "math"
)
func main() {
    var f1 float32
    var f2 float64
    f1 = math.Pi
    f2 = math.Pi

    fmt.Printf("Type: %T Value: %v\n", math.Pi, math.Pi)
    fmt.Printf("Type: %T Value: %v\n", f1, f1)
    fmt.Printf("Type: %T Value: %v\n", f2, f2)
}
```

**输出**

```
Type: float64 Value: 3.141592653589793
Type: float32 Value: 3.1415927
Type: float64 Value: 3.141592653589793
```

请注意上述程序。

+   由于 **math.Pi** 常量的未类型化特性，它可以被赋值给 **float32** 和 **float64** 类型的变量。在类型确定后，GO中通常不允许这样做。

+   当我们打印 **math.Pi** 的类型时，它打印默认类型，即 **float64**。

根据使用情况，无类型常量可以分配给低精度类型（float32）或高精度类型（float64）。

# **命名约定**

常量的命名约定与变量的命名约定相同。

+   常量名称只能以字母或下划线开头。之后可以跟任意数量的字母、数字或下划线。

# **全局常量**

与其他变量一样，如果常量在文件顶部的任何函数的作用域之外声明，则它将在包内全局可用。例如，在下面的程序中，name将是一个在任何函数中都可用的全局常量。请注意，const name在主包外不可用。为了使其在主包外可用，它必须以大写字母开头。

见下面的代码。它还展示了一个包内局部常量的示例。

```
package main

import "fmt"

const name = "test"

func main() {
    const a = 8
    fmt.Println(a)
    testGlobal()
}

func testGlobal() {
    fmt.Println(name)
    //The below line will give compiler error as a is a local constant
    //fmt.Println(a)
```

# **常量类型**

常量可以有四种类型：

+   数值

+   字符串

+   字符

+   布尔型

## **字符串常量**

在Go语言中，字符串常量以两种方式表示

+   任何用双引号括起来的值

+   任何用反引号括起来的值

下面的程序展示了一个示例

+   有类型字符串常量

+   无类型未命名字符串常量

+   无类型命名字符串常量

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

**输出：**

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

在上述程序中，我们在代码中创建了一个新类型**myString**。

```
type myString string
```

以上程序还展示了

+   有类型字符串常量

+   无类型未命名字符串常量

+   无类型命名字符串常量

让我们理解它们及其行为

**有类型字符串常量**

定义如下

```
const aa string = "abc"
```

注意上面这行会导致编译错误。这是因为有类型字符串常量**aa**是**string**类型。因此下面这一行会导致编译错误，因为它不能分配给**myString**类型的变量。

```
var v myString = aa
```

但有类型字符串常量可以分配给用**var**关键字创建的变量，如下所示

```
var uu = aa
```

**无类型命名字符串常量**

定义如下

```
const bb = "abc"
```

无类型命名字符串常量可以分配给**myString**类型的变量，以及用**var**关键字创建的变量，因为它是无类型的，所以常量的类型将根据所分配变量的类型来决定。

```
var ww myString = bb
var xx = bb
```

**无类型未命名字符串常量**。

它是这样的

```
abc
```

无类型未命名字符串常量可以分配给**myString**类型的变量，以及用**var**关键字创建的变量，因为它是无类型的，所以常量的类型将根据所分配变量的类型来决定。

```
var yy myString = "abc"
var zz = "abc"
```

## **数值常量**

数值常量进一步分为三种类型

+   整数

+   浮点数

+   复数

一个无类型整数常量（命名和未命名）可以分配给**int**类型、**float**类型和**complex**类型。这是因为一个int值可以是int、float或complex。例如，int值**123**可以是

+   一个值为123的**int**

+   一个值为123.0的**float**

+   一个虚部为0的**complex**

基于类似逻辑，未类型**float**常量可以赋值给所有**floats**和**complex**类型，但不能赋值给**integer**，因为例如一个float 5.3不能是整数。

基于类似逻辑，未类型**complex**常量可以赋值给**complex**类型，但不能赋值给**integer**和**float**，例如一个float 5i+3不能是**integer**或**float**。

见下面的程序，说明上述观点。在该程序中，我们有一个例子。

+   类型整数常量

+   未类型未命名整数常量

+   未类型命名整数常量

```
package main

import "fmt"

func main() {
	//Typed int constant
	const aa int = 123
	var uu = aa
	fmt.Println("Typed named integer constant")
	fmt.Printf("uu: Type: %T Value: %v\n\n", uu, uu)

	//Below line will raise a compilation error
	//var v int32 = aa

	//Untyped named int constant
	const bb = 123
	var ww = bb
	var xx int32 = bb
	var yy float64 = bb
	var zz complex128 = bb
	fmt.Println("Untyped named integer constant")
	fmt.Printf("ww: Type: %T Value: %v\n", ww, ww)
	fmt.Printf("xx: Type: %T Value: %v\n", xx, xx)
	fmt.Printf("yy: Type: %T Value: %v\n", yy, yy)
	fmt.Printf("zz: Type: %T Value: %v\n\n", zz, zz)

	//Untyped unnamed int constant
	var ll = 123
	var mm int32 = 123
	var nn float64 = 123
	var oo complex128 = 123
	fmt.Println("Untyped unnamed integer constant")
	fmt.Printf("ll: Type: %T Value: %v\n", ll, ll)
	fmt.Printf("mm: Type: %T Value: %v\n", mm, mm)
	fmt.Printf("nn: Type: %T Value: %v\n", nn, nn)
	fmt.Printf("oo: Type: %T Value: %v\n", oo, oo)
}
```

**输出**

```
Typed named integer constant
uu: Type: int Value: 123

Untyped named integer constant
ww: Type: int Value: 123
xx: Type: int32 Value: 123
yy: Type: float64 Value: 123
zz: Type: complex128 Value: (123+0i)

Untyped unnamed integer constant
ll: Type: int Value: 123
mm: Type: int32 Value: 123
nn: Type: float64 Value: 123
oo: Type: complex128 Value: (123+0i)
```

现在上述程序展示了一个

+   类型整数常量

+   未类型未命名整数常量

+   未类型命名整数常量

让我们理解每个常量及其行为

**类型整数常量**

定义如下

```
const aa int = 123
```

类型整数常量可以赋值给使用**var**关键字创建的变量，如下所示。

```
var uu = aa
```

当赋值给另一个**int**类型时，会引发编译错误。因此，下面的代码引发编译错误，因为**aa**变量已经是**int**类型。

```
var v int32 = aa
```

**未类型命名整数常量**

定义如下

```
const bb = 123
```

未类型命名整数常量可以赋值给任何**int**类型、任何**float**类型和任何**complex**数类型，以及使用**var**关键字创建的任何变量。因此，下面的代码可以正常工作。

```
var ww = bb
var xx int32 = bb
var yy float64 = bb
var zz complex128 = bb
```

**未类型未命名整数常量**

如下所示

```
123
```

未类型命名整数常量可以赋值给任何**int**类型、任何**float**类型和任何**complex**数类型，以及使用**var**关键字创建的任何变量。因此，下面的代码可以正常工作。

```
var ww = 123
var xx int32 = 123
var yy float64 = 123
var zz complex128 = 123
```

**数值表达式**

由于未类型常量的特性，不同数值常量类型可以混合搭配形成表达式。

```
package main
import "fmt"
func main() {
    var p = 5.2 / 3
    fmt.Printf("p: Type: %T Value: %v\n", p, p)
}
```

**输出：**

```
p: Type: float64 Value: 1.7333333333333334
```

## **布尔常量**

有两个未类型布尔常量**true**和**false**。下面是一个展示布尔常量的程序。

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

**输出：**

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

在上述程序中，我们创建了一个新类型**myBool**。

```
type myBool bool
```

上述程序展示了一个例子

+   类型布尔常量

+   未类型未命名布尔常量

+   未类型命名布尔常量

让我们理解每个常量及其行为

**类型布尔常量**

定义如下

```
const aa bool = true
```

注意上面的这行会导致编译错误。这是因为变量**aa**是类型为**boolean**的常量。因此，下面这行会导致编译错误，因为无法赋值给类型为**myBool**的变量。

```
var v mybool = aa
```

但类型字符串常量可以赋值给使用**var**关键字创建的变量，如下所示。

```
var uu = aa
```

**未类型命名布尔常量**

定义如下

```
const bb = true
```

未类型命名字符串常量可以赋值给类型为**myBool**的变量以及使用**var**关键字创建的变量，因为它是未类型的，因此常量的类型将根据赋值的变量类型决定。

```
var ww mybool = bb
var xx = bb
```

**未类型未命名布尔常量**

定义如下

```
true
```

未类型化的无名字符串常量可以赋值给**myBool**类型的变量以及使用**var**关键字创建的变量，因为它是无类型的，所以常量的类型将根据所赋值的变量类型决定。

```
var yy mybool = true
var zz = true
```

## **字符常量**

以下是演示字符常量的程序。

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

同时，上述程序展示了示例

+   类型化字符常量

+   未类型化的无名字符常量

+   未类型化的有名字符常量

让我们理解它们每一个及其行为

**类型化字符常量**

它的定义如下

```
const aa int32 = 'a'
```

上述内容中请注意，下面的行将导致编译错误。这是因为变量**aa**是**int32**类型。因此，下面的行将导致编译错误，因为它不能赋值给**myChar**类型的变量。

```
var v myChar = aa
```

但是，类型化字符串常量可以赋值给使用**var**关键字创建的变量，如下所示。

```
var uu = aa
```

**未类型化的有名字符常量**

它的定义如下

```
const bb = 'a'
```

未类型化的有名字符串常量可以赋值给**myChar**类型的变量以及使用**var**关键字创建的变量，因为它是无类型的，所以常量的类型将根据所赋值的变量类型决定。

```
var ww myChar = bb
var xx = bb
```

**未类型化的无名字符常量**。

它如下所示

```
'a'
```

未类型化的无名字符串常量可以赋值给**myChar**类型的变量以及使用**var**关键字创建的变量，因为它是无类型的，所以常量的类型将根据所赋值的变量类型决定。

```
var yy myChar = 'a'
var zz = 'a'
```

# **结论**

这就是关于Golang中const关键字的全部内容。希望你喜欢这篇文章。请在评论中分享反馈。

**下一个教程** – [For Loop](https://golangbyexample.com/for-loop-in-golang/)

**上一个教程** – [Functions](https://golangbyexample.com/function-golang-complete-guide/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
