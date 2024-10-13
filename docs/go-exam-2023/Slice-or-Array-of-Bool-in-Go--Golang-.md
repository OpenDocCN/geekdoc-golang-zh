<!--yml

分类: 未分类

日期: 2024-10-13 06:40:01

-->

# Go（Golang）中的布尔切片或数组

> 来源：[`golangbyexample.com/slice-array-bool-golang/`](https://golangbyexample.com/slice-array-bool-golang/)

目录

**   概述

+   布尔切片

+   布尔数组*  *## **概述**

在 Golang 中，可以创建**bool**数据类型的切片或数组。实际上，可以创建任何数据类型的切片或数组。本教程包含创建布尔数据类型切片或数组的简单示例。

这里需要补充的是，在 golang 中，数组是固定大小的，切片可以有可变大小。更多细节见这里

数组 – [`golangbyexample.com/understanding-array-golang-complete-guide/`](https://golangbyexample.com/understanding-array-golang-complete-guide/)

切片 – [`golangbyexample.com/slice-in-golang/`](https://golangbyexample.com/slice-in-golang/)

## **布尔切片**

```go
package main

import "fmt"

func main() {

	//First Way
	var booleans_first []bool
	booleans_first = append(booleans_first, true)
	booleans_first = append(booleans_first, false)
	booleans_first = append(booleans_first, true)

	fmt.Println("Output for First slice of booleans")
	for _, c := range booleans_first {
		fmt.Println(c)
	}

	//Second Way
	booleans_second := make([]bool, 3)
	booleans_second[0] = false
	booleans_second[1] = true
	booleans_second[2] = false

	fmt.Println("\nOutput for Second slice of booleans")
	for _, c := range booleans_second {
		fmt.Println(c)
	}
}
```

**输出**

```go
Output for First slice of booleans
true
false
true

Output for Second slice of booleans
false
true
false
```

我们有两种创建布尔切片的方法。第一种方法是

```go
var booleans_first []bool
booleans_first = append(booleans_first, true)
booleans_first = append(booleans_first, false)
booleans_first = append(booleans_first, true)
```

第二种方法，我们使用 make 命令创建布尔切片

```go
booleans_second := make([]bool, 3)
booleans_second[0] = false
booleans_second[1] = true
booleans_second[2] = false
```

两种方法都可行。这是我们如何创建布尔切片

## **布尔数组**

```go
package main

import "fmt"

func main() {

	var booleans_first [3]bool

	booleans_first[0] = true
	booleans_first[1] = false
	booleans_first[2] = true

	fmt.Println("Output for First Array of booleans")
	for _, c := range booleans_first {
		fmt.Println(c)
	}

	booleans_second := [3]bool{
		false,
		true,
		false,
	}

	fmt.Println("\nOutput for Second Array of booleans")
	for _, c := range booleans_second {
		fmt.Println(c)
	}
}
```

**输出**

```go
Output for First Array of booleans
true
false
true

Output for Second Array of booleans
false
true
false
```

我们有两种创建数组的方法。第一种方法是

```go
var booleans_first [3]bool
booleans_first[0] = true
booleans_first[1] = false
booleans_first[2] = true
```

在第二种方法中，我们直接用创建的布尔值初始化数组

```go
booleans_second := [3]bool{
	false,
	true,
	false,
}
```

查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力覆盖所有概念和示例。本教程适合那些希望获得专业知识并深入理解 golang 的读者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章适合你 –[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
