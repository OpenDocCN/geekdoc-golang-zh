<!--yml

类别：未分类。

日期：2024-10-13 06:40:11。

-->

# 在 Go (Golang)中创建浮点切片或数组。

> 来源：[`golangbyexample.com/slice-array-floats-golang/`](https://golangbyexample.com/slice-array-floats-golang/)。

目录。

**   概述

+   浮点切片

+   浮点数组

## **概述**

在 Golang 中也可以创建**浮点**数据类型的切片或数组。实际上，任何数据类型都可以在 Go 中创建切片或数组。本教程包含创建浮点数据类型切片或数组的简单示例。

在此补充说明，Golang 中的数组大小是固定的，而切片可以具有可变大小。更多详情请见这里。

数组 – [`golangbyexample.com/understanding-array-golang-complete-guide/`](https://golangbyexample.com/understanding-array-golang-complete-guide/)

切片 – [`golangbyexample.com/slice-in-golang/`](https://golangbyexample.com/slice-in-golang/)。

## **浮点切片**

```go
package main

import "fmt"

func main() {

	//First Way
	var floats_first []float64
	floats_first = append(floats_first, 1.1)
	floats_first = append(floats_first, 2.2)
	floats_first = append(floats_first, 3.3)

	fmt.Println("Output for First slice of floats")
	for _, c := range floats_first {
		fmt.Println(c)
	}

	//Second Way
	floats_second := make([]float64, 3)
	floats_second[0] = 3.3
	floats_second[1] = 2.2
	floats_second[2] = 1.1

	fmt.Println("\nOutput for Second slice of floats")
	for _, c := range floats_second {
		fmt.Println(c)
	}
}
```

**输出**

```go
Output for First slice of floats
1.1
2.2
3.3

Output for Second slice of floats
3.3
2.2
1.1
```

我们有两种创建浮点切片的方法。第一种方法是

```go
var floats_first []float64
floats_first = append(floats_first, 1.1)
floats_first = append(floats_first, 2.2)
floats_first = append(floats_first, 3.3)
```

在第二种方法中，我们使用 make 命令来创建浮点切片。

```go
floats_second := make([]float64, 3)
floats_second[0] = 3.3
floats_second[1] = 2.2
floats_second[2] = 1.1
```

## **浮点数组**

```go
package main

import "fmt"

func main() {

	var floats_first [3]float64

	floats_first[0] = 1.1
	floats_first[1] = 2.2
	floats_first[2] = 3.3

	fmt.Println("Output for First Array of floats")
	for _, c := range floats_first {
		fmt.Println(c)
	}

	floats_second := [3]float64{
		3.3,
		2.2,
		1.1,
	}

	fmt.Println("\nOutput for Second Array of floats")
	for _, c := range floats_second {
		fmt.Println(c)
	}
}
```

**输出**

```go
Output for First Array of floats
1.1
2.2
3.3

Output for Second Array of floats
3.3
2.2
1.1
```

我们有两种创建数组的方法。第一种方法是

```go
var floats_first [3]float64
floats_first[0] = 1.1
floats_first[1] = 2.2
floats_first[2] = 3.3
```

在第二种方法中，我们直接用创建的浮点数初始化数组。

```go
floats_second := [3]float64{
	3.3,
	2.2,
	1.1,
}
```

请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力涵盖所有概念和示例。本教程适合希望获得专业知识和对 Golang 有深入理解的读者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

如果您有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章适合您 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)。


