<!--yml

分类：未分类。

日期：2024-10-13 06:28:19。

-->

# Go (Golang) 中的数字/整数/浮点常量。

> 来源：[https://golangbyexample.com/integer-constant-golang/](https://golangbyexample.com/integer-constant-golang/)

目录。

**[概述](#Overview "概述")**。

+   [示例](#Example "示例")。

    +   [类型化整数常量](#Typed_integer_constant "类型化整数常量")。

    +   [未类型命名的整数常量](#Untyped_named_integer_constant "未类型命名的整数常量")。

    +   [未类型未命名的整数常量](#Untyped_unnamed_integer_constant "未类型未命名的整数常量")。

+   [数字表达式](#Numeric_Expressions "数字表达式")*  *# **概述**。

Go 中的数字常量进一步分为三种类型。

+   整数。

+   浮点数。

+   复数。

为了更好地理解 Go 中的数字常量，理解类型化和未类型常量非常重要。请参考这篇文章 – [https://golangbyexample.com/typed-untyped-constant-golang](https://golangbyexample.com/typed-untyped-constant-golang)。

一旦你阅读完本文，你会理解常量可以以三种方式声明。

+   类型化常量。

+   未类型未命名常量。

+   未类型命名常量。

数字的情况也是一样。

未类型的整数常量（命名和未命名）可以赋值给 **int** 类型、**float** 类型和 **complex**。这是因为一个 int 值可以是 int 或 float 或 complex。例如，整数值 **123** 可以是。

+   一个值为 123 的 **int**。

+   一个值为 123.0 的 **float**。

+   一个虚部为 0 的 **complex**。

根据类似的逻辑，未类型的 **float** 常量可以赋值给所有 **floats** 和 **complex** 类型，但不能赋值给 **integer**，因为例如浮点数 5.3 不能是整数。

根据类似的逻辑，未类型的 **complex** 常量可以赋值给 **complex** 类型，但不能赋值给 **integer** 和 **float**，因为例如浮点数 5i+3 不能是 **integer** 或 **float**。

让我们看看一个程序来理解它。

# **示例**。

请参见下面的程序说明上述观点。在程序中我们有例子。

+   类型化整数常量。

+   未类型未命名的整数常量。

+   未类型命名的整数常量。

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

**输出**。

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

现在上面的程序展示了一个例子。

+   类型化整数常量。

+   未类型未命名的整数常量。

+   未类型命名的整数常量。

让我们了解每种方式及其行为。

## **类型化整数常量**。

它定义如下。

```
const aa int = 123
```

类型化整数常量可以赋值给使用 **var** 关键字创建的变量，如下所示。

```
var uu = aa
```

当赋值给另一个 **int** 类型时会引发编译错误。因此下面的代码会引发编译错误，因为 **aa** 变量已经是 **int** 类型。

```
var v int32 = aa
```

## **未类型命名的整数常量**。

它定义如下。

```
const bb = 123
```

未类型命名的整数常量可以赋值给任何 **int** 类型、任何 **float** 类型和任何 **complex** 数字类型，以及使用 **var** 关键字创建的任何变量。因此下面的代码是可行的。

```
var ww = bb
var xx int32 = bb
var yy float64 = bb
var zz complex128 = bb
```

## **未类型未命名的整数常量**。

它如下所示。

```
123
```

无类型命名整数常量可以赋值给任何**int**类型、任何**float**类型以及任何**complex**数字类型，也可以赋值给任何使用**var**关键字创建的变量。因此，下面的代码是可行的。

```
var ww = 123
var xx int32 = 123
var yy float64 = 123
var zz complex128 = 123
```

# **数值表达式**

由于常量数值的无类型特性，不同的数值常量类型可以混合使用以形成一个表达式。

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

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)*
