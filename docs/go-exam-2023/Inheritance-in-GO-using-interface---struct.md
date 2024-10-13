<!--yml

分类：未分类

日期：2024-10-13 06:00:45

-->

# 使用接口 + 结构体的 GO 继承

> 来源：[https://golangbyexample.com/inheritance-go-interface-struct/](https://golangbyexample.com/inheritance-go-interface-struct/)

本文描述了使用接口和结构体的继承。请查看我们关于 Go 继承的完整指南以获取全面参考。

> [OOP: GOLANG 继承完整指南](https://golangbyexample.com/oop-inheritance-golang-complete/)

[https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=EpVuUFYEMN#?secret=OtvYyDmPmR](https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=EpVuUFYEMN#?secret=OtvYyDmPmR)

Go 通过嵌入结构体或使用接口来支持继承。实现方式不同，每种方式都有一些限制。这些不同的方法包括：

1.  通过使用嵌入结构体 – 父结构体嵌入在子结构体中。这个方法的限制是无法进行子类型化。你不能将子结构体传递给期望基础结构体的函数。有关更多详细信息，请参考这个链接 – [**使用结构体的继承**](https://golangbyexample.com/inheritance-go-struct/)

1.  通过使用接口 – 可以进行子类型化，但限制是无法引用公共属性。有关更多详细信息，请参考这个链接 – [**使用接口的继承**](https://golangbyexample.com/inheritance-go-interface/)

1.  **通过使用接口 + 结构体** – 这解决了上述两种方法的限制，但一个限制是无法重写方法。不过有变通办法。当前文章描述了这种方法。

**详细信息：**

在这种方法中，基础结构体嵌入在子结构体中，基础结构体实现了所有公共接口的方法。因此，子结构体可以：

1.  访问基础结构体的方法和属性

1.  由于基础结构体实现了公共接口的所有函数，公共接口本身可以用于子类型化。

```go
package main

import "fmt"

type iBase interface {
	say()
}

type base struct {
	value string
}

func (b *base) say() {
	fmt.Println(b.value)
}

type child struct {
	base  //embedding
	style string
}

func check(b iBase) {
	b.say()
}

func main() {
	base := base{value: "somevalue"}
	child := &child{
		base:  base,
		style: "somestyle",
	}
	child.say()
	check(child)
} 
```

**输出：**

```go
somevalue
somevalue 
```
