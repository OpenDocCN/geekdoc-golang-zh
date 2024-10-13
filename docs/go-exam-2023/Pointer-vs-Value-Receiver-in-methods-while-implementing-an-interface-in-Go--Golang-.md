<!--yml

类别：未分类

日期：2024-10-13 06:22:21

-->

# 在Go（Golang）中实现接口时，指针与值接收器的方法

> 来源：[https://golangbyexample.com/pointer-vs-value-receiver-method-golang/](https://golangbyexample.com/pointer-vs-value-receiver-method-golang/)

一个类型的方法可以有指针接收器或值接收器。当该类型实现接口时，指针与值接收器有一些注意事项

+   如果一个类型使用值接收器实现了接口的所有方法，那么该类型的值和指针都可以用于赋值给该接口变量或传递给接受该接口作为参数的函数。

+   如果一个类型使用指针接收器实现了接口的所有方法，那么只有该类型的指针可以用于赋值给该接口变量或传递给接受该接口作为参数的函数。

示例以演示上述第一个要点

假设我们有一个接口**动物**如下

```
type animal interface {
    breathe()
    walk()
}
```

我们也有一个**狮子**结构体实现了这个**动物**接口

```
type lion struct {
    age int
}
```

**代码**

```
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

    a = lion{age: 10}
    a.breathe()
    a.walk()

    a = &lion{age: 5}
    a.breathe()
    a.walk()
}
```

**输出**

```
Lion breathes
Lion walk
Lion breathes
Lion walk
```

**狮子**结构体使用值接收器实现了**动物**接口。因此它适用于**狮子**类型的两个变量以及指向**狮子**类型变量的指针。

这样可以

```
 a = lion{age: 10}
```

以及这个

```
a = &lion{age: 5}
```

示例以演示上述第二个要点。**狮子**结构体使用指针接收器实现了**动物**接口。因此它仅适用于指向**狮子**类型变量的指针。

所以这样可以

```
a = &lion{age: 5}
```

但这会导致编译错误

```
a = lion{age: 10}
```

```
cannot use lion literal (type lion) as type animal in assignment:
        lion does not implement animal (breathe method has pointer receiver)
```

查看完整工作代码

```
package main

import "fmt"

type animal interface {
	breathe()
	walk()
}

type lion struct {
	age int
}

func (l *lion) breathe() {
	fmt.Println("Lion breathes")
}

func (l *lion) walk() {
	fmt.Println("Lion walk")
}

func main() {
	var a animal

	//a = lion{age: 10}
	a.breathe()
	a.walk()

	a = &lion{age: 5}
	a.breathe()
	a.walk()
}
```

取消注释这一行

```
a = lion{age: 10}
```

这将导致编译错误

```
cannot use lion literal (type lion) as type animal in assignment:
        lion does not implement animal (breathe method has pointer receiver)
```

+   [go](https://golangbyexample.com/tag/go/)
