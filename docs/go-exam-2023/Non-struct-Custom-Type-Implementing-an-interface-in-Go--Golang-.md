<!--yml

类别：未分类

日期：2024-10-13 06:22:52

-->

# 非结构体自定义类型在Go（Golang）中实现接口

> 来源：[https://golangbyexample.com/non-struct-type-implementing-interface-go/](https://golangbyexample.com/non-struct-type-implementing-interface-go/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# **概述**

任何非结构体自定义类型实现接口也是完全可以的。让我们来看一个例子。

假设我们有一个接口**animal**如下

```
type animal interface {
    breathe()
    walk()
}
```

# **代码**

```
package main

import "fmt"

type animal interface {
	breathe()
	walk()
}

type cat string

func (c cat) breathe() {
	fmt.Println("Cat breathes")
}

func (c cat) walk() {
	fmt.Println("Cat walk")
}

func main() {
	var a animal

	a = cat("smokey")
	a.breathe()
	a.walk()
}
```

**输出**

```
Cat breathes
Cat walk
```

上述程序说明了任何自定义类型也可以实现接口的概念。**cat**是字符串类型，它实现了**breathe**和**walk**方法，因此将**cat**类型的实例赋值给**animal**类型的变量是正确的。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
