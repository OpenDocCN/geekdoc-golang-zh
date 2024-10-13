<!--yml

类别：未分类

日期：2024-10-13 06:28:06

-->

# Go 中的字符串常量（Golang）

> 来源：[`golangbyexample.com/string-constant-golang/`](https://golangbyexample.com/string-constant-golang/)

目录

**概述**

+   示例

    +   有类型的字符串常量

    +   未类型化的命名字符串常量

    +   未类型化的未命名字符串常量 *  *# **概述**

在 Go 中，字符串常量以两种方式表示

+   任何用双引号括起来的值

+   任何用反引号括起来的值

为了更好地理解 Golang 中的字符串常量，了解 Go 中的有类型和无类型常量是重要的。请参阅这篇文章：[`golangbyexample.com/typed-untyped-constant-golang/`](https://golangbyexample.com/typed-untyped-constant-golang/)

一旦你阅读了这篇文章，你会理解常量可以以三种方式声明

+   有类型常量

+   未类型化的未命名常量

+   未类型化的命名常量

字符串的情况也是如此。让我们看一个程序来理解它。

# **示例**

以下程序展示了一个示例

+   有类型的字符串常量

+   未类型化的未命名字符串常量

+   未类型化的命名字符串常量

```go
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

```go
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

```go
type myString string
```

上述程序还展示了一个示例

+   有类型的字符串常量

+   未类型化的未命名字符串常量

+   未类型化的命名字符串常量

让我们理解它们的每一种及其行为

## **有类型的字符串常量**

定义如下

```go
const aa string = "abc"
```

上述内容注意，下面的行将导致编译错误。这是因为类型字符串常量**aa**的类型是**string**。因此，下面的行将导致编译错误，因为它无法分配给类型为**myString**的变量。

```go
var v myString = aa
```

但是，有类型的字符串常量可以分配给用**var**关键字创建的变量，如下所示

```go
var uu = aa
```

## **未类型化的命名字符串常量**

定义如下

```go
const bb = "abc"
```

未类型化的命名字符串常量可以分配给类型为**myString**的变量，以及用**var**关键字创建的变量，因为它是未类型化的，因此常量的类型将根据被分配的变量的类型决定。

```go
var ww myString = bb
var xx = bb
```

## **未类型化的未命名字符串常量**

它如下所示

```go
abc
```

未类型化的未命名字符串常量可以分配给类型为**myString**的变量，以及用**var**关键字创建的变量，因为它是未类型化的，因此常量的类型将根据被分配的变量的类型决定。

```go
var yy myString = "abc"
var zz = "abc"
```

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)*
