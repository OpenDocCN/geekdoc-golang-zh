<!--yml

类别：未分类

日期：2024-10-13 06:43:09

-->

# 在Go (Golang)中将一个切片附加或添加到另一个切片

> 来源：[https://golangbyexample.com/append-one-slice-another-golang/](https://golangbyexample.com/append-one-slice-another-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

也可以将一个切片追加到另一个切片。下面是该操作的格式。

```
res := append(slice1, slice2...)
```

注意第二个切片后的**‘…’**。**‘…’**是一个运算符，表示参数是一个可变参数。这意味着在运行时，slice2将被扩展为其各个元素，并作为多个参数传递给append函数。

## **程序**

```
package main

import "fmt"

func main() {
    numbers1 := []int{1, 2}
    numbers2 := []int{3, 4}
    numbers := append(numbers1, numbers2...)
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**输出**

```
numbers=[1 2 3 4]
length=4
capacity=4
```

**注意：** 请查看我们的Golang高级教程。本系列的教程内容详尽，我们尽力覆盖所有概念并附上示例。本教程适合那些希望获得专业知识和对golang有深刻理解的人——[Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对如何在Golang中实现所有设计模式感兴趣。如果是，那么这篇文章适合你——[所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
