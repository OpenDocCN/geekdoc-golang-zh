<!--yml

类别：未分类

日期：2024-10-13 06:26:37

-->

# Go (Golang) 中的恢复函数示例

> 来源：[https://golangbyexample.com/recover-example-go/](https://golangbyexample.com/recover-example-go/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

Go提供了一个内置函数**recover**用于从恐慌中恢复。下面是这个函数的签名

```go
func recover() interface{}
```

**defer**函数是唯一在**panic**之后被调用的函数。因此，把**recover**函数放在**defer**函数中是有意义的。如果**recover**函数不在defer函数内，则无法停止**panic**。

# **例子**

让我们看一个recover的例子

```go
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

```go
Recovering from panic: Out of bound access for slice
Exiting normally
```

在上面的程序中，我们有一个函数**checkAndPrint**，它检查并打印传递到参数中的索引处的切片元素。如果传递的索引大于数组的长度，则程序将会恐慌。我们在**checkAndPrint**函数的开始处添加了一个名为**handleOutOfBounds**的defer函数。这个函数包含对recover函数的调用，如下所示。

```go
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

**recover**函数将捕获恐慌，我们还可以打印恐慌中的消息。

```go
Recovering from panic: Out of bound access for slice
```

在恢复函数之后，程序继续运行，控制权返回到调用的函数，这里是**main**。这就是为什么我们得到输出为

```go
Exiting normally
```

recover函数返回传递给panic函数的值。因此，检查recover函数的返回值是一个好习惯。如果返回值非nil，则没有发生恐慌，并且recover函数没有与恐慌一起被调用。这就是我们在defer函数**handleOutOfBounds**中有以下代码的原因。

```go
if r := recover(); r != nil 
```

如果**r**为nil，那么没有发生恐慌。所以如果没有恐慌，则对recover的调用将返回nil。

注意，如果defer函数和recover函数没有从恐慌函数中调用，则在这种情况下，恐慌也可以在被调用的函数中恢复。实际上，可能在调用栈的后续链中恢复。

让我们看一个示例。

```go
package main

import "fmt"

func main() {
    a := []string{"a", "b"}
    checkAndPrintWithRecover(a, 2)
    fmt.Println("Exiting normally")
}
func checkAndPrintWithRecover(a []string, index int) {
    defer handleOutOfBounds()
    checkAndPrint(a, 2)
}
func checkAndPrint(a []string, index int) {
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

```go
Recovering from panic: Out of bound access for slice
Exiting normally
```

上面的程序与之前的程序几乎相同，唯一的不同是我们有一个额外的函数**checkAndPrintWithRecover**，其中包含对

+   带有recover的defer函数是**handleOutOfBounds**

+   调用**checkAndPrint**函数

**基本上，**checkAndPrint**函数引发恐慌，但没有recover函数，而是调用recover的地方在**checkAndPrintWithRecover**函数中。但程序仍然能够从恐慌中恢复，因为恐慌也可以在被调用的函数中恢复，并且可以在链中继续恢复。**

***   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)***
