<!--yml

分类：未分类

日期：2024-10-13 06:34:22

-->

# 如何在Go（Golang）中初始化具有数组或切片字段的结构。

> 来源：[https://golangbyexample.com/struct-slice-field-go/](https://golangbyexample.com/struct-slice-field-go/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

一个结构可以包含一个字段，该字段是另一种类型的切片或数组。要初始化这样的结构，我们可以首先初始化另一种类型的切片/数组。之后，我们可以初始化父结构。在下面的示例中，**class**结构类型有一个**student**结构类型的切片。

```go
type student struct {
	name   string 
	rollNo int    
	city   string 
}

type class struct {
	className string
	students  []student
}
```

要初始化这种类型的结构，我们需要首先初始化嵌套结构的切片。也就是说，我们将首先初始化**student**结构的切片。例如如下所示。

```go
goerge := student{"Goerge", 35, "Newyork"}
john := student{"Goerge", 25, "London"}

students := []student{goerge, john}
```

然后我们可以如下初始化**class**结构。

```go
class := class{"firstA", students}class := class{"firstA", students}
```

另一种方法是在初始化**class**结构时直接初始化**student**结构的切片。如下所示。

```go
class := class{"firstA", []student{goerge, john}}
```

# **程序**

让我们看一下完整的程序。

```go
package main

import "fmt"

type class struct {
	className string
	students  []student
}

type student struct {
	name   string
	rollNo int
	city   string
}

func main() {
	goerge := student{"Goerge", 35, "Newyork"}
	john := student{"Goerge", 25, "London"}

	students := []student{goerge, john}
	class := class{"firstA", students}

	fmt.Printf("class is %v\n", class)
}
```

**输出**

```go
class is {firstA [{Goerge 35 Newyork} {Goerge 25 London}]}
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
