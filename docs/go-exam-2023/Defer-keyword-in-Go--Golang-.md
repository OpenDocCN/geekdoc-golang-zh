<!--yml

分类：未分类

日期：2024-10-13 06:25:41

-->

# Go 语言中的**defer**关键字

> 来源：[`golangbyexample.com/defer-golang/`](https://golangbyexample.com/defer-golang/)

这是 Golang 综合教程系列的第十四章。请参考此链接获取系列的其他章节 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [Pointer](https://golangbyexample.com/pointer-golang/)

**上一个教程** – [Switch](https://golangbyexample.com/switch-statement-golang/)

现在让我们来看一下当前的教程。下面是当前教程的目录。

目录

**   概述

+   defer 中的自定义函数

+   defer 中的内联函数

+   defer 是如何工作的

+   延迟参数评估

+   同一函数中的多个 defer 函数

+   defer 函数和命名返回值

+   defer 和方法

+   defer 和 panic

+   结论*  *# **概述**

**defer**（延迟）顾名思义用于延迟函数中的清理操作。这些清理操作将在函数结束时执行。这些清理操作将在一个由**defer**调用的不同函数中完成。这个不同的函数会在周围函数返回之前被调用。下面是**defer**函数的语法。

```go
defer {function_or_method_call}
```

关于 defer 函数需要注意的事项

+   延迟函数的执行会被推迟到周围函数返回的时刻。

+   如果封闭函数异常终止，延迟函数也会被执行。例如在发生 panic 的情况下。

理解**defer**函数的一个好例子是查看写入文件的使用案例。一个为写入而打开的文件也必须关闭。

```go
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    file.Close()
    return nil
}
```

在上述程序中，在**writeToTempFile**函数中，我们打开一个文件，然后尝试向文件写入一些内容。在我们写入文件内容后关闭文件。在写入操作期间，可能会导致错误，函数会在不关闭文件的情况下返回。**Defer**函数帮助避免这些问题。**Defer**函数总是在周围函数返回之前执行。让我们在这里重写上述程序，使用**defer**函数。

```go
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    defer file.Close()

    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    return nil
}
```

在上述程序中，我们在打开文件后执行**defer file.Close()**。这将确保即使写入文件时发生错误，文件也会被关闭。`defer`函数确保文件会被关闭，无论函数中有多少个返回语句。

# **自定义函数在 defer 中**

我们也可以在**defer**中调用自定义函数。让我们看看一个例子。

```go
package main
import "fmt"
func main() {
    defer test()
    fmt.Println("Executed in main")
}
func test() {
    fmt.Println("In Defer")
}
```

**输出**

```go
Executed in main
In Defer
```

在上述程序中，有一个**defer**语句调用名为**test**的自定义函数。从输出中可以看到，**test**函数在主函数中的所有内容执行完毕后、主函数返回之前被调用。这就是原因。

```go
Executed in main
```

被打印在前。

```go
In Defer
```

上述函数还显示在主函数中使用`defer`是完全可以的。

# **defer 中的内联函数**

也可以在`defer`中使用内联函数。让我们看看一个例子。

```go
package main

import "fmt"

func main() {
    defer func() { fmt.Println("In inline defer") }()
    fmt.Println("Executed")
}
```

**输出**

```go
Executed
In inline defer
```

在上述代码中，我们有一个带内联函数的**defer**。

```go
defer func() { fmt.Println("In inline defer") }()
```

在 Go 中这是允许的。此外，请注意在函数后添加**“()”**是强制性的，否则编译器会抛出错误。

```go
expression in defer must be function call
```

从输出中可以看到，内联函数在主函数中的所有内容执行完毕后、主函数返回之前被调用。这就是原因。

```go
Executed in main
```

被打印在前。

```go
In inline Defer
```

# **defer 是如何工作的**

当编译器在函数中遇到`defer`语句时，它将其推送到一个列表中。该列表在内部实现了一个堆栈数据结构。所有在同一函数中遇到的`defer`语句都被推送到这个列表中。当外层函数返回时，堆栈中的所有函数从上到下执行，然后才能开始调用函数的执行。调用函数中也会发生同样的事情。

让我们了解当多个`defer`函数在不同函数中时会发生什么。想象一下从**main**函数调用到**f1**函数，再到**f2**函数。

**main**->**f1**->**f2**

以下是`f2`返回后将发生的顺序。

+   如果存在，**f2**中的`defer`函数将被执行。控制将返回到调用者，即**f1**函数。

+   如果存在，**f1**中的`defer`函数将被执行。控制将返回到调用者，即**main**函数。注意，如果中间还有更多函数，处理过程将以类似方式继续向上堆栈。

+   在主函数返回后，如果在主函数中存在`defer`函数，将被执行。

让我们看看一个程序。

```go
package main

import "fmt"

func main() {
	defer fmt.Println("Defer in main")
	fmt.Println("Stat main")
	f1()
	fmt.Println("Finish main")
}

func f1() {
	defer fmt.Println("Defer in f1")
	fmt.Println("Start f1")
	f2()
	fmt.Println("Finish f1")
}

func f2() {
	defer fmt.Println("Defer in f2")
	fmt.Println("Start f2")
	fmt.Println("Finish f2")
}
```

**输出**

```go
Stat main
Start f1
Start f2
Finish f2
Defer in f2
Finish f1
Defer in f1
Finish main
Defer in main
```

# **defer 参数的评估**

`defer`参数在`defer`语句被求值时被评估。

让我们看看一个程序。

```go
package main

import "fmt"

func main() {
	sample := "abc"

	defer fmt.Printf("In defer sample is: %s\n", sample)
	sample = "xyz"
}
```

**输出**

```go
In defer sample is: abc
```

在上述程序中，当`defer`语句被评估时，**sample**变量的值为**“abc”**。在`defer`函数中，我们打印`sample`变量。在`defer`语句之后，我们将**sample**变量的值更改为**“xyz”**。但程序输出的是**“abc”**而不是**“xyz”**，因为当`defer`参数被评估时，**sample**变量的值是**“abc”**。

# **同一函数中的多个 defer 函数**

如果在特定函数中有多个 defer 函数，那么所有的 defer 函数将按照后进先出顺序执行。

让我们来看一个程序。

```go
package main
import "fmt"
func main() {
    i := 0
    i = 1
    defer fmt.Println(i)
    i = 2
    defer fmt.Println(i)
    i = 3
    defer fmt.Println(i)
}
```

**输出**

```go
3
2
1
```

在上面的程序中，我们有三个 **defer** 函数，每个函数打印变量 **i** 的值。变量 **i** 在每个 defer 之前递增。代码首先输出 3，这意味着第三个 defer 函数首先执行。然后输出 2，表示第二个 defer 之后执行，最后输出 1，意味着第一个 defer 最后执行。这表明在特定函数中存在多个 defer 函数时，它们遵循“后进先出”规则。因此程序输出。

```go
3
2
1
```

# **延迟函数和命名返回值**

在函数中，如果有命名返回值，defer 函数可以读取并修改这些命名返回值。如果 defer 函数修改了命名返回值，则将返回该修改后的值。

让我们来看一个程序。

```go
package main
import "fmt"
func main() {
    s := test()
    fmt.Println(s)
}
func test() (size int) {
    defer func() { size = 20 }()
    size = 30
    return
}
```

**输出**

```go
20
```

在上面的程序中，我们在测试函数中有命名返回值 **“size”**。在 defer 函数中，我们修改了命名返回值，并将值改为 20。然后我们将 size 设置为 30。在主函数中，我们打印测试函数的返回值，结果输出 20 而不是 30，因为 defer 函数在测试函数中修改了 size 变量的值。

# **延迟和方法**

**defer** 语句同样适用于方法，类似于它对函数的适用。在第一个示例中，我们已经看到在文件实例上调用的 **Close** 方法。这表明 **defer** 语句同样适用于方法。

# **延迟和恐慌**

即使在程序中发生了恐慌，defer 函数也会被执行。当在某个函数中触发 panic 时，该函数的执行将被停止，任何被延迟的函数将被执行。实际上，栈中所有函数调用的延迟函数也会被执行，直到所有函数都返回。此时程序将退出，并打印 panic 消息。

所以如果存在一个 defer 函数，它将被执行，控制将返回给调用函数，调用函数如果存在 defer 函数，则会再次执行，链条将继续，直到程序退出。

让我们来看一个示例。

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

在上面的程序中，我们首先有一个 defer 函数，然后手动触发 panic。正如你从输出中看到的，defer 函数得到了执行，输出中打印了以下行。

```go
Defer in main
```

# **结论**

这就是 Golang 中的 defer。希望你喜欢这篇文章，请在评论中分享反馈/改进/错误。

**下一个教程** – [指针](https://golangbyexample.com/pointer-golang/)

**上一个教程** – [切换](https://golangbyexample.com/switch-statement-golang/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
