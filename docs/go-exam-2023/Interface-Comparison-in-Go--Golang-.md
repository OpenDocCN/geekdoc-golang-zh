<!--yml

类别：未分类

日期：2024-10-13 06:22:11

-->

# Go (Golang) 中的接口比较

> 来源：[`golangbyexample.com/interface-comparison-golang/`](https://golangbyexample.com/interface-comparison-golang/)

为了理解两个接口变量是否相等，我们首先需要了解接口的内部表示。像其他变量一样，接口变量由类型和值表示。接口变量的值，实际上包含两个元组

+   基本类型

+   基本值

请参见下面的图示，说明我们上面提到的内容

![](img/e09464771d53171817cfeecf9da6a93e.png)

两个接口可比较，如果任一

+   接口值为 nil 或

+   基本类型相同且可比较。基本值也是相同的

根据 Go 规范，一些可比较的类型包括

+   布尔型

+   数值型

+   字符串，

+   指针

+   通道

+   接口类型

+   结构体 – 如果它的所有字段类型都是可比较的

+   数组 – 如果数组元素的值类型是可比较的

根据 Go 规范，一些不可比较的类型

+   切片

+   映射

+   函数

两个接口变量可以使用**==**或**!=**运算符进行比较

让我们看看一个程序

```go
package main

import "fmt"

type animal interface {
	breathe()
	walk()
}

type lion struct {
	age int
}

func (l lion) breathe() {
	fmt.Println("Lion breathes")
}

func (l lion) walk() {
	fmt.Println("Lion walk")
}

func main() {
	var a animal
	var b animal
	var c animal
	var d animal
	var e animal

	a = lion{age: 10}
	b = lion{age: 10}
	c = lion{age: 5}

	if a == b {
		fmt.Println("a and b are equal")
	} else {
		fmt.Println("a and b are not equal")
	}

	if a == c {
		fmt.Println("a and c are equal")
	} else {
		fmt.Println("a and c are not equal")
	}

	if d == e {
		fmt.Println("d and e are equal")
	} else {
		fmt.Println("d and e are not equal")
	}
}
```

**输出**

```go
a and b are equal
a and c are not equal
d and e are equal
```

在上述程序中，我们有动物接口，**lion**结构体通过定义其两个方法来实现动物接口。

接口变量**a**和**b**相等，因为

+   基本类型相同，即**lion**结构体

+   基本类型是可比较的，即**lion**结构体只有一个**int**类型的字段，而**int**类型是可比较的

+   基本值相同，即**lion.age**对于两者都是 10

前两点同样适用于比较 a 和 c，但它们不相等，因为

+   基本值不相同，即**lion.age**对于**a**是 10，而对于**c**是 5

**接口变量 d 和 e 相等，因为**

***   接口值对两者都是 nil


