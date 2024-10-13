<!--yml

分类：未分类

日期：2024-10-13 06:28:02

-->

# 在Go（Golang）中声明常量

> 来源：[https://golangbyexample.com/declaring-constant-go/](https://golangbyexample.com/declaring-constant-go/)

常量是指任何不会改变其值的东西。在Go中，const可以是字符串、数字、布尔值和字符类型。

常量可以使用**const**关键字声明。需要注意的重要一点是，值必须在声明常量时赋值。这与变量不同，变量的值可以稍后赋值。

+   声明**const**并指定类型 – 它以**const**关键字开头，后接名称，然后是类型。值也必须立即赋值，如上所述。

```
const c string = "circle"
```

+   不指定类型声明常量 – 没有类型声明的const是无类型常量。我们稍后会深入了解有类型和无类型常量。目前，重要的是要知道，无类型声明的const具有默认的隐藏类型。常量在以任何方式（直接初始化、传递给函数等）赋值给变量时会被赋予一个类型。

```
const c = "circle"
```

+   一起声明多个const

```
const (
  c = "circle"
  s = "square"
)
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
