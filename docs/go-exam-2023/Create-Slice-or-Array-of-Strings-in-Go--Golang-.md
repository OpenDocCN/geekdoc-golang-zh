<!--yml

分类：未分类

日期：2024-10-13 06:40:16

-->

# 在Go (Golang)中创建字符串切片或数组

> 来源：[https://golangbyexample.com/slice-array-string-golang/](https://golangbyexample.com/slice-array-string-golang/)

目录

**   [概述](#Overview "概述")

+   [字符串切片](#Slice_of_String "字符串切片")

+   [字符串数组](#Array_of_Strings "字符串数组")*  *## **概述**

在Golang中，也可以创建**string**数据类型的切片或数组。实际上，Go中可以创建任何数据类型的切片或数组。本教程包含创建**string**数据类型的切片或数组的简单示例。

这里补充一下，在golang中，数组是固定大小的，而切片可以有可变大小。更多详细信息请查看这里。

数组 – [https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

切片 – [https://golangbyexample.com/slice-in-golang/](https://golangbyexample.com/slice-in-golang/)

## **字符串切片**

```
package main

import "fmt"

func main() {

	//First Way
	var string_first []string
	string_first = append(string_first, "abc")
	string_first = append(string_first, "def")
	string_first = append(string_first, "ghi")

	fmt.Println("Output for First slice of string")
	for _, c := range string_first {
		fmt.Println(c)
	}

	//Second Way
	string_second := make([]string, 3)
	string_second[0] = "ghi"
	string_second[1] = "def"
	string_second[2] = "abc"

	fmt.Println("\nOutput for Second slice of string")
	for _, c := range string_second {
		fmt.Println(c)
	}
}
```

**输出**

```
Output for First slice of string
abc
def
ghi

Output for Second slice of string
ghi
def
abc
```

我们有两种创建字符串切片的方法。第一种方法是

```
var string_first []string
string_first = append(string_first, "abc")
string_first = append(string_first, "def")
string_first = append(string_first, "ghi")
```

在第二种方法中，我们使用make命令来创建字符串切片。

```
string_second := make([]string, 3)
string_second[0] = "ghi"
string_second[1] = "def"
string_second[2] = "abc"
```

无论哪种方式都可以。这是我们创建字符串切片的方法。

## **字符串数组**

```
package main

import "fmt"

func main() {

	var string_first [3]string

	string_first[0] = "abc"
	string_first[1] = "def"
	string_first[2] = "ghi"

	fmt.Println("Output for First Array of string")
	for _, c := range string_first {
		fmt.Println(c)
	}

	string_second := [3]string{
		"ghi",
		"def",
		"abc",
	}

	fmt.Println("\nOutput for Second Array of string")
	for _, c := range string_second {
		fmt.Println(c)
	}
}
```

**输出**

```
Output for First Array of string
abc
def
ghi

Output for Second Array of string
ghi
def
abc
```

我们有两种创建数组的方法。第一种方法是

```
var string_first [3]string
string_first[0] = "abc"
string_first[1] = "def"
string_first[2] = "ghi"
```

在第二种方法中，我们直接用一些字符串初始化数组。

```
string_second := [3]string{
	"ghi",
	"def",
	"abc",
}
```

查看我们的Golang高级教程。本系列的教程内容详尽，尽力涵盖所有概念及示例。本教程适合那些希望掌握golang并深入理解的读者 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章就是为你准备的 – [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
