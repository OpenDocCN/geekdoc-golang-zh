<!--yml

类别：未分类

日期：2024-10-13 06:26:06

-->

# Go 语言中的 Panic 和 Recover

> 来源：[https://golangbyexample.com/panic-and-recover-golang/](https://golangbyexample.com/panic-and-recover-golang/)

这是第 28 章，也是 Go 语言综合教程系列的最后一章。请参考此链接获取该系列的其他章节 – [Go 语言综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**上一个教程** – [错误 - 第 2 部分](https://golangbyexample.com/error-in-golang-advanced/)

现在让我们来查看当前的教程。下面是当前教程的目录。

目录

**[概述](#Overview "概述")**

+   [运行时错误 Panic](#Runtime_Error_Panic "运行时错误 Panic")

+   [显式调用 panic 函数](#Calling_the_panic_function_explicitly "显式调用 panic 函数")

+   [使用 defer 进行 Panic](#Panic_with_defer "使用 defer 进行 Panic")

+   [在 Go 语言中恢复](#Recover_in_golang "在 Go 语言中恢复")

+   [Panic/Recover 和 Goroutine](#PanicRecover_and_Goroutine "Panic/Recover 和 Goroutine")

+   [打印堆栈跟踪](#Printing_stack_trace "打印堆栈跟踪")

+   [当 panic 被恢复时函数的返回值](#Return_value_of_the_function_when_panic_is_recovered "当 panic 被恢复时函数的返回值")

+   [结论](#Conclusion "结论") * *# **概述**

Go 语言中的 panic 类似于异常。panic 旨在在异常条件下退出程序。panic 可以通过两种方式在程序中发生

+   程序中的运行时错误

+   通过显式调用 panic 函数。当程序无法继续并且必须退出时，程序员可以调用此函数

Go 提供了一种特殊的函数来创建 panic。以下是该函数的语法

```go
func panic(v interface{})
```

该函数可以被程序员显式调用以创建 panic。它接受一个空接口作为参数。当程序发生 panic 时，它输出两件事

+   传递给 panic 函数的错误信息作为参数

+   panic 发生时的堆栈跟踪

# **运行时错误 Panic**

程序中的运行时错误可能发生在以下情况下

+   超出范围的数组访问

+   在 nil 指针上调用函数

+   在关闭的通道上发送

+   不正确的类型断言

让我们来看一个由于超出范围的数组访问导致的运行时错误示例。

```go
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	print(a, 2)
}

func print(a []string, index int) {
	fmt.Println(a[index])
}
```

**输出**

```go
panic: runtime error: index out of range [2] with length 2

goroutine 1 [running]:
main.checkAndPrint(...)
        main.go:12
main.main()
        /main.go:8 +0x1b
exit status 2
```

在上面的程序中，我们有一个长度为 2 的切片，我们试图在 **print** 函数中访问索引为 3 的切片。超出边界的访问是不允许的，这将引发 panic，如输出所示。请注意，输出中有两件事

+   错误信息

+   panic 发生时的堆栈跟踪

在程序中可能发生运行时错误的情况还有很多。我们不会提到所有情况，但你可以理解大概

# **显式调用 panic 函数**

程序员可以显式调用 panic 函数的一些情况有：

+   该函数期望一个有效的参数，但却传入了 nil 参数。在这种情况下，程序无法继续，将为传入的 nil 参数抛出 panic。

+   任何其他无法继续的场景。

让我们来看一个例子。

```go
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
}

func checkAndPrint(a []string, index int) {
	if index > (len(a) - 1) {
		panic("Out of bound access for slice")
	}
	fmt.Println(a[index])
}
```

**输出**

```go
panic: Out of bound access for slice

goroutine 1 [running]:
main.checkAndPrint(0xc000104f58, 0x2, 0x2, 0x2)
      main.go:13 +0xe2
main.main()
        main.go:8 +0x7d
exit status 2
```

在上述程序中，我们再次有一个函数 **checkAndPrint**，它接受一个切片作为参数和一个索引。然后它检查传入的索引是否大于切片长度减去 1。如果是，则为切片的越界访问，因此会抛出 panic。如果不是，则打印该索引处的值。请注意，在输出中有两件事情。

+   错误消息

+   panic 发生位置的堆栈跟踪

# **使用 defer 的 Panic**

当函数中抛出 panic 时，该函数的执行将停止，任何已延迟的函数将被执行。实际上，堆栈中所有函数调用的延迟函数也会被执行，直到所有函数都返回。此时程序将退出，并打印 panic 消息。

如果存在 defer 函数，它将被执行，控制将返回给调用函数，如果调用函数中也有 defer 函数，则会再次执行，链条将继续，直到程序退出。

让我们来看一个例子。

```go
package main
import "fmt"
func main() {
    defer fmt.Println("Defer in main")
    panic("Panic with Defer")
    fmt.Println("After painc in f2")
}
```

**输出**

```go
Defer in main
panic: Panic Create

goroutine 1 [running]:
main.main()
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/deferWithPanic/main.go:7 +0x95
exit status 2
```

在上述程序中，我们首先有一个 defer 函数，然后我们手动触发 panic。正如你在输出中看到的，defer 函数得到了执行，下面的行在输出中被打印。

```go
Defer in main
```

让我们理解当程序中发生 panic 时会发生什么。想象一下从 **main** 函数到 **f1** 函数再到 **f2** 函数的函数调用。

**main**->**f1**->**f2**

现在假设在函数 **f2** 中发生了 panic，那么接下来将发生的事件顺序如下。

+   **f2** 的执行将停止。如果 **f2** 中有 defer 函数，它将被执行。控制将返回给调用者，也就是函数 **f1**。

+   **f1** 函数将以类似的方式行为，仿佛在该函数中发生了 panic，然后控制将返回给调用者，也就是 **main** 函数。请注意，如果中间有更多函数，过程将以类似方式继续向上堆栈。

+   **main** 函数的行为将如同在该函数中发生了 panic，之后程序将崩溃。

+   一旦程序崩溃，它将打印 panic 消息以及该堆栈跟踪。

让我们看看一个程序。

```go
package main
import "fmt"
func main() {
    f1()
}
func f1() {
    defer fmt.Println("Defer in f1")
    f2()
    fmt.Println("After painc in f1")
}
func f2() {
    defer fmt.Println("Defer in f2")
    panic("Panic Demo")
    fmt.Println("After painc in f2")
}
```

**输出**

```go
Defer in f2
Defer in f1
panic: Panic Demo

goroutine 1 [running]:
main.f2()
        main.go:17 +0x95
main.f1()
        main.go:11 +0x96
main.main()
        main.go:6 +0x20
exit status 2
```

在上述程序中，panic 在 **f2** 函数中发生，如下所示。

```go
panic("Panic Demo")
```

**f2** 中的 defer 函数在此之后被调用，并打印以下消息。

```go
Defer in f2
```

请注意，一旦 **f2** 函数中发生 panic，其执行将停止，因此下面的代码行将不会执行 **f2**。

```go
fmt.Println("After painc in f2")
```

控制返回到 **f1**，如果它有 defer 函数，则会执行该 defer 函数，并打印以下消息。

```go
Defer in f1
```

控制权随后返回到main函数，然后程序崩溃。输出打印了panic消息以及从main到f1再到f2的整个堆栈跟踪。

# **在Golang中使用Recover**

Go提供了一个内置函数**recover**用于从panic中恢复。以下是该函数的签名

```go
func recover() interface{}
```

我们已经了解到，**defer**函数是唯一在**panic**之后被调用的函数。因此，将**recover**函数放在**defer**函数中是合理的。如果**recover**函数不在defer函数内，则不会停止**panic**。

让我们来看一个recover的例子

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

在上面的程序中，我们有一个函数**checkAndPrint**，它检查并打印传入参数中的切片元素。如果传入的索引大于数组的长度，则程序将引发panic。我们在函数**checkAndPrint**的开始处添加了一个名为**handleOutIfBounds**的defer函数。此函数包含如下的recover函数调用。

```go
if r := recover(); r != nil {
    fmt.Println("Recovering from panic:", r)
}
```

**recover**函数将捕获panic，我们还可以打印来自panic的消息。

```go
Recovering from panic: Out of bound access for slice
```

在recover函数之后，程序继续运行，控制权返回到调用的函数，即这里的**main**。这就是为什么我们会得到如下输出

```go
Exiting normally
```

recover函数返回传递给panic函数的值。因此，检查recover函数的返回值是一种好习惯。如果返回值为nil，则表示没有发生panic，且recover函数没有与panic一起被调用。这就是为什么在defer函数**handleOutOfBounds**中有以下代码。

```go
if r := recover(); r != nil 
```

如果**r**为nil，则表示没有发生panic。因此，如果没有panic，则对recover的调用将返回nil。

请注意，如果defer函数和recover函数不是从引发panic的函数中调用的，那么在被调用函数中也可以恢复panic。实际上，可以在调用栈的后续链中恢复panic。

让我们来看这个的一个例子。

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

上面的程序与前一个程序非常相似，唯一不同的是我们添加了一个额外的函数**checkAndPrintWithRecover**，其中包含对该函数的调用。

+   使用**handleOutOfBounds**的defer函数与recover

+   调用**checkAndPrint**函数

**基本上，**checkAndPrint**函数引发panic，但没有recover函数，而是对recover的调用在**checkAndPrintWithRecover**函数中。但是，程序仍然能够从panic中恢复，因为panic也可以在被调用的函数中以及随后在链中恢复。**

**我们上面提到，如果recover函数不在defer函数中，则不会停止panic。**

让我们来看一个示例程序

```go
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
	fmt.Println("Exiting normally")
}

func checkAndPrint(a []string, index int) {
	handleOutOfBounds()
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
panic: Out of bound access for slice

goroutine 1 [running]:
main.checkAndPrint(0xc000104f58, 0x2, 0x2, 0x2)
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/recoverNegativeExample/main.go:15 +0xea
main.main()
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/recoverNegativeExample/main.go:8 +0x81
exit status 2
```

在上面的程序中，recover函数不在defer函数中。如您所见，输出表明它没有停止panic，因此您看到了上面的输出。

# **Panic/Recover与Goroutine**

关于 recover 函数需要注意的一个重要点是，它只能恢复同一 goroutine 中发生的 panic。如果 panic 发生在不同的 goroutine 中，而 recover 在另一个 goroutine 中，那么它不会停止 panic。让我们看一个程序来演示这一点。

```go
package main
import "fmt"
func main() {
    a := []string{"a", "b"}
    checkAndPrintWithRecover(a, 2)
    time.Sleep(time.Second)
    fmt.Println("Exiting normally")
}
func checkAndPrintWithRecover(a []string, index int) {
    defer handleOutOfBounds()
    go checkAndPrint(a, 2)
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
Exiting normally
panic: Out of bound access for slice

goroutine 18 [running]:
main.checkAndPrint(0xc0000a6020, 0x2, 0x2, 0x2)
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/goroutine/main.go:19 +0xe2
created by main.checkAndPrintWithRecover
        /Users/slohia/go/src/github.com/golang-examples/articles/tutorial/panicRecover/goroutine/main.go:14 +0x82
exit status 2
```

在上面的程序中，我们在 goroutine 中有 **checkAndPrint**，它在该 goroutine 中引发了 **panic**。**recover** 函数在调用的 goroutine 中。如你从输出中看到的，它并没有停止 **panic**，因此你看到了上面的输出。

# **打印堆栈跟踪**

golang 的 Debug 包还提供 StackTrace 函数，可以用来在 recover 函数中打印 panic 的堆栈跟踪。

```go
package main
import (
    "fmt"
    "runtime/debug"
)
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
        fmt.Println("Stack Trace:")
        debug.PrintStack()
    }
}
```

**输出**

```go
Recovering from panic: Out of bound access for slice
Stack Trace:
goroutine 1 [running]:
runtime/debug.Stack(0xd, 0x0, 0x0)
        stack.go:24 +0x9d
runtime/debug.PrintStack()
        stack.go:16 +0x22
main.handleOutOfBounds()
        main.go:27 +0x10f
panic(0x10ab8c0, 0x10e8f60)
        /Users/slohia/Documents/goversion/go1.14.1/src/runtime/panic.go:967 +0x166
main.checkAndPrint(0xc000104f58, 0x2, 0x2, 0x2)
        main.go:18 +0x111
main.main()
        main.go:11 +0x81
Exiting normally
```

在上面的程序中，我们使用 **StackTrace** 函数在 recover 函数中打印 panic 的堆栈跟踪。它打印出的堆栈跟踪是正确的，如输出所示。

# **当 panic 被恢复时函数的返回值**

当 panic 被恢复时，导致 panic 的函数的返回值将是该函数返回类型的默认值。

让我们看一个程序来演示这一点。

```go
package main
import (
    "fmt"
)
func main() {
    a := []int{5, 6}
    val, err := checkAndGet(a, 2)
    fmt.Printf("Val: %d\n", val)
    fmt.Println("Error: ", err)
}
func checkAndGet(a []int, index int) (int, error) {
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    return a[index], nil
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
Val: 0
Error: 
```

在上面的程序中，我们有一个 **checkAndGet** 函数，它在 int 切片的特定索引获取值。如果传递给此函数的索引大于（切片长度 - 1），那么它会引发 panic。还有一个 **handleOutOfBounds** 函数用于从 panic 中恢复。因此，我们将索引 2 传递给 **checkAndGet** 函数，它引发的 panic 在 **handleOutOfBounds** 函数中恢复。这就是我们首先得到这个输出的原因。

```go
Recovering from panic: Out of bound access for slice
```

请注意在主函数中，我们以这样的方式重新获取 **checkAndGet** 的返回值。

```go
val, err := checkAndGet(a, 2)
```

**checkAndGet** 有两个返回值

+   int

+   错误

由于 **checkAndGet** 创建的 panic 在 handleOutOfBounds 函数中被恢复，因此 **checkAndGet** 的返回值将是其类型的默认值。

因此

```go
 fmt.Printf("Val: %d\n", val)
```

输出

```go
Val: 0
```

因为零是 **int** 类型的默认值。

而且

```go
fmt.Println("Error: ", err)
```

输出

```go
Error: 
```

因为 nil 是 **error** 类型的默认值。

如果你不想返回类型的默认零值，可以使用命名返回值。让我们看一个程序来演示这一点。

```go
package main
import (
    "fmt"
)
func main() {
    a := []int{5, 6}
    val, err := checkAndGet(a, 2)
    fmt.Printf("Val: %d\n", val)
    fmt.Println("Error: ", err)
}
func checkAndGet(a []int, index int) (value int, err error) {
    value = 10
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    value = a[index]
    return value, nil
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
Val: 10
Error: 
```

这个程序与之前的程序相同，唯一的区别是我们在 **checkAndGet** 函数中使用了命名返回值。

```go
func checkAndGet(a []int, index int) (value int, err error)
```

我们在 **checkAndGet** 函数中将命名返回值设置为 10。

```go
value = 10
```

这就是为什么在这个程序中我们得到下面的输出，因为引发了 panic 并且它被恢复。

```go
Recovering from panic: Out of bound access for slice
Val: 10
Error: 
```

还请注意，如果程序中没有引发 panic，那么它将输出索引的正确值。

# **结论**

这就是关于 golang 中的 panic 和 recover 的全部内容。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**上一个教程** – [错误 - 第 2 部分](https://golangbyexample.com/error-in-golang-advanced/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) ***
