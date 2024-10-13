<!--yml

分类：未分类

日期：2024-10-13 06:40:06

-->

# 在 Go 中创建整数的切片或数组（Golang）

> 来源：[`golangbyexample.com/slice-array-integers-golang/`](https://golangbyexample.com/slice-array-integers-golang/)

目录

**   概述**

+   整数切片

+   整数数组*  *## **概述**

在 Golang 中也可以创建**int**数据类型的切片或数组。实际上，Go 中可以创建任何数据类型的切片或数组。本教程包含简单的示例，演示如何在 Golang 中创建 int 数据类型的切片或数组。

这里补充一下，在 Golang 中数组是固定大小的，而切片可以有可变大小。更多细节在这里

数组 – [`golangbyexample.com/understanding-array-golang-complete-guide/`](https://golangbyexample.com/understanding-array-golang-complete-guide/)

切片 – [`golangbyexample.com/slice-in-golang/`](https://golangbyexample.com/slice-in-golang/)

## **整数切片**

```go
package main

import "fmt"

func main() {

	//First Way
	var integers_first []int
	integers_first = append(integers_first, 1)
	integers_first = append(integers_first, 2)
	integers_first = append(integers_first, 3)

	fmt.Println("Output for First slice of integers")
	for _, c := range integers_first {
		fmt.Println(c)
	}

	//Second Way
	integers_second := make([]int, 3)
	integers_second[0] = 3
	integers_second[1] = 2
	integers_second[2] = 1

	fmt.Println("\nOutput for Second slice of integers")
	for _, c := range integers_second {
		fmt.Println(c)
	}
}
```

**输出**

```go
Output for First slice of integers
1
2
3

Output for Second slice of integers
3
2
1
```

我们有两种创建整数切片的方法。第一种方法是

```go
var integers_first []int
integers_first = append(integers_first, 1)
integers_first = append(integers_first, 2)
integers_first = append(integers_first, 3)
```

在第二种方法中，我们使用 make 命令来创建整数切片

```go
integers_second := make([]int, 3)
integers_second[0] = 3
integers_second[1] = 2
integers_second[2] = 1
```

无论哪种方法都可以。这就是我们如何创建整数切片

## **整数数组**

```go
package main

import "fmt"

func main() {

	var integers_first [3]int

	integers_first[0] = 1
	integers_first[1] = 2
	integers_first[2] = 3

	fmt.Println("Output for First Array of integers")
	for _, c := range integers_first {
		fmt.Println(c)
	}

	integers_second := [3]int{
		3,
		2,
		1,
	}

	fmt.Println("\nOutput for Second Array of integers")
	for _, c := range integers_second {
		fmt.Println(c)
	}
}
```

**输出**

```go
Output for First Array of integers
1
2
3

Output for Second Array of integers
3
2
1
```

我们有两种创建数组的方法。第一种方法是

```go
var integers_first [3]int
integers_first[0] = 1
integers_first[1] = 2
integers_first[2] = 3
```

在第二种方法中，我们直接用创建的整数初始化数组

```go
integers_second := [3]int{
	3,
	2,
	1,
}
```

查看我们的 Golang 高级教程。本系列教程详尽，我们努力覆盖所有概念及示例。本教程适合那些希望获得专业知识和扎实理解 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章适合你 –[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
