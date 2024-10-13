<!--yml

分类：未分类

日期：2024-10-13 06:00:33

-->

# 使用结构体（嵌入）的GO中的继承

> 来源：[https://golangbyexample.com/inheritance-go-struct/](https://golangbyexample.com/inheritance-go-struct/)

本文仅描述使用结构体的继承。请查看我们的《Go语言中的继承完整指南》以获取完整参考。

> [面向对象：GOLANG中的继承完整指南](https://golangbyexample.com/oop-inheritance-golang-complete/)

[https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=z8WWaJ1OiI#?secret=zH9oZkEbS3](https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=z8WWaJ1OiI#?secret=zH9oZkEbS3)

Go支持通过嵌入结构体或使用接口进行继承。实现方式不同，每种方式都有一些限制。不同的方式有：

1.  **通过使用嵌入结构体** – 父结构体嵌入到子结构体中。这个方法的限制是子类型不可能。你不能将子结构体传递给期望基结构体的函数。当前帖子描述了这种方法。

1.  通过使用接口 – 子类型是可能的，但限制是无法引用公共属性。有关更多详情，请参考此链接 – [**使用接口的继承**](https://golangbyexample.com/inheritance-go-interface/)

1.  通过使用接口 + 结构体 – 这解决了上述两种方法的限制，但一个限制是无法重写方法。不过有一个解决方法。有关更多详情，请参考此链接 – [**使用接口 + 结构体的继承**](https://golangbyexample.com/inheritance-go-interface-struct/)

**详情：**

在使用结构体的继承中，基结构体被嵌入到子结构体中，基属性和方法可以直接在子结构体上调用。请参见下面的代码：

```
package main

import "fmt"

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

func check(b base) {
	b.say()
}

func main() {
	base := base{value: "somevalue"}
	child := &child{
		base:  base,
		style: "somestyle",
	}
	child.say()
	//check(child)
} 
```

**输出：**

```
somevalue
```

**限制：**

不支持子类型。你不能将子结构体传递给期望基结构体的函数。

例如，在上述代码中，如果你取消注释**//check(child)**，将会出现编译错误：“无法将child（类型*child）作为参数传递给check，期望类型为base”。要解决此问题，我们可以通过接口进行继承。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [inheritance](https://golangbyexample.com/tag/inheritance/)*   [interface](https://golangbyexample.com/tag/interface/)*   [struct](https://golangbyexample.com/tag/struct/)
