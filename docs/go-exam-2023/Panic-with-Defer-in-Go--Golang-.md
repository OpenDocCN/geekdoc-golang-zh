<!--yml

类别：未分类

日期：2024-10-13 06:25:35

-->

# 在 Go 中使用 Defer 处理 Panic (Golang)

> 来源：[https://golangbyexample.com/panic-with-defer-golang/](https://golangbyexample.com/panic-with-defer-golang/)

目录

**   [概述](#Overview "概述")

+   [示例](#Example "示例")

+   [使用 defer 从 panic 中恢复](#Using_defer_to_recover_from_a_panic "使用 defer 从 panic 中恢复")*  *# **概述**

即使程序发生 **panic**，**defer** 函数也会被执行。事实上，**defer** 函数是唯一在 **panic** 之后被调用的函数。当在一个函数中引发 **panic** 时，该函数的执行将停止，任何被延迟的函数将被执行。实际上，所有堆栈中的延迟函数也将被执行，直到所有函数都返回。此时，程序将退出，并打印 panic 消息。

因此，如果存在 defer 函数，它将被执行，控制将返回到调用函数，如果存在的话，调用函数将再次执行其 defer 函数，这个链条将继续，直到程序退出。

# **示例**

让我们看一个例子。

```
package main

import "fmt"

func main() {
    defer fmt.Println("Defer in main")
    panic("Panic with Defer")
    fmt.Println("After painc in f2")
}
```

**输出**

```
Defer in main
panic: Panic Create

goroutine 1 [running]:
main.main()
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/deferWithPanic/main.go:7 +0x95
exit status 2
```

在上述程序中，我们首先有一个 **defer** 函数，然后手动引发 panic。正如你从输出中看到的，defer 函数被执行，输出中打印了如下行。

```
Defer in main
```

# **使用 defer 从 panic 中恢复**

Go 提供了一个内置函数 **recover** 用于从 panic 中恢复。下面是这个函数的签名。

```
func recover() interface{}
```

**defer** 函数是唯一在 **panic** 之后被调用的函数。因此，将 **recover** 函数放在 **defer** 函数中是合理的。如果 **recover** 函数不在 defer 函数中，它将无法停止 **panic**。

让我们看看 recover 的一个例子。

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

在上述程序中，我们有一个函数 **checkAndPrint**，它检查并打印传入参数的索引处的切片元素。如果传入的索引大于数组的长度，则程序会发生 panic。我们在函数 **checkAndPrint** 的开头添加了一个名为 **handleOutIfBounds** 的 defer 函数。这个函数包含了对 **recover** 函数的调用，如下所示。

```
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

**recover** 函数将捕获 panic，我们还可以打印 panic 中的消息。

```
Recovering from panic: Out of bound access for slice
```

在 **recover** 函数之后，程序继续执行，控制返回到调用的函数，即这里的 **main**。这就是我们获得输出的原因。

```
Exiting normally
```

+   [去](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
