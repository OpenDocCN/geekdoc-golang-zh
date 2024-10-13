<!--yml

类别：未分类

日期：2024-10-13 06:39:51

-->

# Go（Golang）中的映射切片

> 来源：[https://golangbyexample.com/slice-map-golang/](https://golangbyexample.com/slice-map-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

在Golang中也可以创建映射数据类型的切片。事实上，Go中可以创建任何数据类型的切片。下面是创建映射切片的一个简单示例

## **程序**

```go
package main

import "fmt"

func main() {
	maps := make([]map[string]string, 3)

	map1 := make(map[string]string)
	map1["1"] = "a"

	map2 := make(map[string]string)
	map2["2"] = "b"

	map3 := make(map[string]string)
	map3["3"] = "c"

	maps[0] = map1
	maps[1] = map2
	maps[2] = map3

	for _, m := range maps {
		fmt.Println(m)
	}
}
```

**输出**

```go
map[1:a]
map[2:b]
map[3:c]
```

在上述程序中，我们创建了三种类型为**map[string]string**的映射

```go
map1 := make(map[string]string)
map1["1"] = "a"

map2 := make(map[string]string)
map2["2"] = "b"

map3 := make(map[string]string)
map3["3"] = "c"
```

我们也像这样创建了一个映射数据类型的切片

```go
maps := make([]map[string]string, 3)
```

这就是我们如何创建一个映射切片

查看我们的Golang高级教程。本系列教程内容详尽，我们试图通过示例覆盖所有概念。这个教程是为那些希望获得专业知识和对Golang有深入理解的人准备的 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，这篇文章适合你 –[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
