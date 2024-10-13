<!--yml

分类：未分类

日期：2024-10-13 06:26:17

-->

# Go（Golang）中的`recover`函数返回值

> 来源：[https://golangbyexample.com/recover-return-value-golang/](https://golangbyexample.com/recover-return-value-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example") *  *# **概述**

`recover`函数返回传递给`panic`函数的值。因此，检查`recover`函数的返回值是一种良好实践。如果返回值非空，则表示没有发生`panic`，且`recover`函数没有因`panic`被调用。

# **示例**

让我们看一个程序以充分理解它

```
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
	fmt.Println("Exiting normally")
}

func checkAndPrint(a []string, index int) {
	defer handleOutOfBounds()
	if index > (len(a) - 1) {
		panic("Out of bound access for slice")
	}
	fmt.Println(a[index])
}

func handleOutOfBounds() {
	if r := recover(); r != nil {
		fmt.Println("Recovering from panic:", r)
	}
}
```

**输出**

```
Recovering from panic: Out of bound access for slice
Exiting normally
```

在上面的程序中，我们有一个函数**checkAndPrint**，它检查并打印传递给参数的索引处的切片元素。如果传递的索引大于数组的长度，则程序将发生`panic`。我们在函数**checkAndPrint**的开始处添加了一个名为**handleOutIfBounds**的`defer`函数。此函数包含对`recover`函数的调用，如下所示。

```
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

**recover**函数将捕获`panic`，我们也可以打印来自`panic`的消息。

```
Recovering from panic: Out of bound access for slice
```

在`recover`函数之后，程序继续执行，控制权返回到调用的函数，这里是**main**。这就是我们得到输出的原因。

```
Exiting normally
```

`recover`函数返回传递给`panic`函数的值。这就是我们在`defer`函数**handleOutofBounds**中有下面代码的原因。

```
if r := recover(); r != nil 
```

在这里，如果**r**为nil，则表示没有发生`panic`。因此，如果没有`panic`，则对`recover`的调用将返回nil。

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
