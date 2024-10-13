<!--yml 

category: 未分类 

date: 2024-10-13 06:43:56 

--> 

# 在 Go 中对字符串进行排序(Golang) 

> 来源：[`golangbyexample.com/sort-string-golang/`](https://golangbyexample.com/sort-string-golang/) 

目录 

**   概述*  *## **概述** 

在 Golang 中，字符串是字节的序列。字符串字面量实际上表示一个 UTF-8 字节序列。在 UTF-8 中，ASCII 字符是单字节，对应前 128 个 Unicode 字符。所有其他字符的字节数在 1 到 4 之间。要更好地理解，请考虑下面的字符串 

```go
sample := "a£c"
```

在上述字符串中 

+   ‘a’根据 UTF-8 编码占用一个字节 

+   ‘£’根据 UTF-8 编码占用两个字节 

+   ‘b’根据 UTF-8 编码占用一个字节 

上述字符串总共占用 1+2+1 = 4 个字节。因此，当我们使用标准的**len()**函数打印字符串长度时，它将输出 4，而不是 3，因为**len()**函数返回字符串中的字节数。 

```go
fmt.Printf("Length is %d\n", len(sample))
```

因此，为了对字符串进行排序，我们需要将其转换为 rune 数组，然后使用 Go 的**sort.Sort**函数对其进行排序。 

以下是**sort.Sort**函数的签名 

```go
func Sort(data Interface)
```

这里是**接口**的定义 

```go
type Interface interface {
	// Len is the number of elements in the collection.
	Len() int

	// Less reports whether the element with index i
	// must sort before the element with index j.
	Less(i, j int) bool

	// Swap swaps the elements with indexes i and j.
	Swap(i, j int)
}
```

因此，无论我们想用**sort.Sort**函数排序什么，都需要实现上述三个函数。

+   Len() int 

+   Less(i, j int) bool 

+   Swap(i, j int) 

在下面的程序中，我们做了同样的事情 

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	sortString("bac")
}

func sortString(input string) {
	runeArray := []rune(input)
	sort.Sort(sortRuneString(runeArray))
	fmt.Println(string(runeArray))
}

type sortRuneString []rune

func (s sortRuneString) Swap(i, j int) {
	s[i], s[j] = s[j], s[i]
}

func (s sortRuneString) Less(i, j int) bool {
	return s[i] < s[j]
}

func (s sortRuneString) Len() int {
	return len(s)
}
```

**输出** 

```go
abc
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力涵盖所有概念并附上示例。本教程适合那些希望获得专业知识和扎实理解 Golang 的读者 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/) 

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/) 

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)* 
