<!--yml

类别：未分类

日期：2024-10-13 06:28:24

-->

# Go (Golang)中的布尔常量

> 来源：[`golangbyexample.com/boolean-constant-go/`](https://golangbyexample.com/boolean-constant-go/)

目录

**概述**

+   示例

    +   类型布尔常量

    +   未类型命名布尔常量

    +   未类型未命名布尔常量 

# **概述**

有两个未类型的布尔常量**true**和**false**。下面是一个说明布尔常量的程序。

为了更好地理解 Golang 中的布尔常量，了解 Go 中的类型和未类型常量是很重要的。请参考这篇文章 – [`golangbyexample.com/typed-untyped-constant-golang`](https://golangbyexample.com/typed-untyped-constant-golang)/

一旦你读完这篇文章，你就会明白常量可以以三种方式声明。

+   类型常量

+   未类型未命名常量

+   未类型命名常量

布尔常量的情况也是如此。让我们看看一个程序来理解它。

# **示例**

```go
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

```go
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

```go
type myBool bool
```

以上程序还展示了

+   类型布尔常量

+   未类型未命名布尔常量

+   未类型命名布尔常量

让我们理解它们及其行为

## **类型布尔常量**

它的定义如下

```go
const aa bool = true
```

上述注意到，以下代码行将导致编译错误。这是因为变量**aa**是类型为**boolean**的常量。因此，以下代码行会导致编译错误，因为它无法被赋值给类型为**myBool**的变量。

```go
var v mybool = aa
```

但类型字符串常量可以像下面那样赋值给用**var**关键字创建的变量。

```go
var uu = aa
```

## **未类型命名布尔常量**

它的定义如下

```go
const bb = true
```

未类型命名字符串常量可以赋值给类型为**myBool**的变量以及用**var**关键字创建的变量，因为它是未类型的，因此常量的类型将根据赋值给的变量类型决定。

```go
var ww mybool = bb
var xx = bb
```

## **未类型未命名布尔常量**

它类似于下面的情况。

```go
true
```

未类型未命名字符串常量可以赋值给类型为**myBool**的变量以及用**var**关键字创建的变量，因为它是未类型的，因此常量的类型将根据赋值给的变量类型决定。

```go
var yy mybool = true
var zz = true
```


