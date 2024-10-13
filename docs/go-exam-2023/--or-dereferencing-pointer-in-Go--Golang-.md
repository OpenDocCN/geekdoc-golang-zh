<!--yml

分类：未分类

日期：2024-10-13 06:28:55

-->

# * 或在 Go (Golang) 中解引用指针

> 来源：[`golangbyexample.com/dereferencing-pointer-golang/`](https://golangbyexample.com/dereferencing-pointer-golang/)

目录

+   概述

+   程序 

# **概述**

***** 运算符可以用来：

+   解引用指针意味着获取存储在指针地址中的值。

+   还可以改变该指针位置的值

# **程序**

```go
package main

import "fmt"

func main() {
	a := 2
	b := &a
	fmt.Println(a)
	fmt.Println(*b)

	*b = 3
	fmt.Println(a)
	fmt.Println(*b)

	a = 4
	fmt.Println(a)
	fmt.Println(*b)
}
```

**输出**

```go
2
2
3
3
4
4
```

**a** 和 ***b** 在内部引用同一个变量。因此，改变一个的值会反映在另一个上。并且，***** 和 **&** 也可以一起使用，但它们会相互抵消。

因此，下面的两个是等价的，并将打印 2

+   a

+   *&a

下面的三个是等价的，并将打印存储在 b 变量中的变量 a 的地址

+   b

+   *&b

+   &*b

注意： *a 不是有效的操作，因为 a 不是指针


