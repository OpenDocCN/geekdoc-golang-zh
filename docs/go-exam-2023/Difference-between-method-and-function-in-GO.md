<!--yml

类别：未分类

日期：2024-10-13 06:04:52

-->

# GO 中方法和函数的区别

> 来源：[`golangbyexample.com/difference-between-method-function-go/`](https://golangbyexample.com/difference-between-method-function-go/)

方法和函数之间有一些重要的区别。让我们来看一下两者的签名。

**功能：**

```go
func some_func_name(arguments) return_values
```

**方法：**

```go
func (receiver receiver_type) some_func_name(arguments) return_values
```

从上面的签名来看，方法有一个接收者参数。接收者可以是一个结构体或任何其他类型。该方法将可以访问接收者的属性，并可以调用接收者的其他方法。

这是函数和方法之间唯一的区别，但正因如此，它们在功能上有所不同。

+   函数可以作为一阶对象使用并可以传递，而方法则不能。

+   方法可以在接收者上进行链式调用，而函数则不能用于相同的目的。

+   可以存在具有相同名称但接收者不同的方法，但在同一包中不能存在两个具有相同名称的不同函数。

+   [函数](https://golangbyexample.com/tag/function/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [方法](https://golangbyexample.com/tag/method/)
