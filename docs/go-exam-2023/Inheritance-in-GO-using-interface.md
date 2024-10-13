<!--yml

类别：未分类

日期：2024-10-13 06:00:38

-->

# 使用接口的 GO 中的继承

> 来源：[`golangbyexample.com/inheritance-go-interface/`](https://golangbyexample.com/inheritance-go-interface/)

本文描述了使用接口的继承。请访问我们的 Go 中的继承完整指南文章以获取完整参考

> [OOP：GOLANG 中的继承完整指南](https://golangbyexample.com/oop-inheritance-golang-complete/)

[`golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=lwzs553n8n#?secret=ZXUMNIsae2`](https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=lwzs553n8n#?secret=ZXUMNIsae2)

Go 通过嵌入结构体或使用接口支持继承。有不同的实现方式，每种方式都有一些限制。这些不同的方式是：

1.  通过使用嵌入式结构体——父结构体嵌入到子结构体中。这个方法的限制是无法进行子类型化。你不能将子结构体传递给期望基类的函数。更多详情请参见此链接——**[使用结构体的继承](https://golangbyexample.com/inheritance-go-struct/)**

1.  **通过使用接口**——子类型化是可能的，但限制是无法引用公共属性。当前的文章描述了这种方法

1.  通过使用接口 + 结构体——这解决了上述两种方法的限制，但一个限制是无法重写方法。不过有一个变通方法。更多详情请参见此链接——**[使用接口 + 结构体的继承](https://golangbyexample.com/inheritance-go-interface-struct/)**

**详情：**

子结构体实现公共接口的方法。这个方法也解决了子类型化的问题。见以下代码

```go
package main

import "fmt"

type iBase interface {
	say()
}

type child struct {
	style string
}

func (b *child) say() {
	fmt.Println(b.style)
}

func check(b iBase) {
	b.say()
}

func main() {
	child := &child{
		style: "somestyle",
	}
	child.say()
	check(child)
} 
```

**输出：**

```go
somestyle
somestyle
```

**限制：**

这种方法的限制在于，无法引用公共属性，因为接口不能有任何属性。这个问题通过使用结构体 + 接口的混合方法得到解决。


