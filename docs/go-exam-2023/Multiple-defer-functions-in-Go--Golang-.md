<!--yml

类别：未分类

日期：2024-10-13 06:27:13

-->

# Go（Golang）中的多个延迟函数

> 来源：[`golangbyexample.com/multiple-defer-functions-golang/`](https://golangbyexample.com/multiple-defer-functions-golang/)

目录

**概述**

+   在特定函数中的多个延迟函数

+   在不同函数中的多个延迟函数

# **概述**

多个延迟函数有两种情况。

+   在特定函数中的多个延迟函数

+   在不同函数中的多个延迟函数

在我们看到这两者的示例之前，让我们先看看延迟函数是如何工作的。当编译器在一个函数中遇到延迟语句时，它会将其推入一个列表中。这个列表内部实现了一个类似栈的数据结构。所有在同一个函数中遇到的延迟语句都将被推入这个列表。当外部函数返回时，栈中的所有函数（从上到下）将在调用函数的执行开始之前被执行。现在，调用函数中也会发生同样的事情。

现在让我们看一下这两者的示例。

# **在特定函数中的多个延迟函数**

如果我们在某个特定函数中有多个延迟函数，那么所有的延迟函数将按照后进先出（last in first out）的顺序执行，这与我们上面提到的类似。

让我们看一下相关的程序。

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

在上面的程序中，我们有三个**延迟（defer）**函数，每个函数打印变量**i**的值。变量**i**在每个延迟函数之前递增。代码首先输出 3，这意味着第三个延迟函数是第一个执行的。然后输出 2，意味着第二个延迟函数在此之后执行，最后输出 1，意味着第一个延迟函数是最后执行的。这表明，当一个特定函数中有多个延迟函数时，它们遵循“后进先出”规则。因此程序输出：

```go
3
2
1
```

# **在不同函数中的多个延迟函数**

让我们理解当我们在不同的函数中有多个延迟（defer）函数时会发生什么……想象从**主**函数调用**f1**函数，再到**f2**函数的场景。

**主**->**f1**->**f2**

下面是**f2**返回后将发生的顺序。

+   如果存在，**f2**中的延迟函数将会被执行。控制将返回给调用者，即**f1**函数。

+   如果存在，**f1**中的延迟函数将会被执行。控制将返回给调用者，即**主**函数。请注意，如果中间有更多的函数，过程将以类似的方式继续向上执行栈。

+   当主函数返回时，主函数中的延迟函数（如果存在）将被执行。

让我们看一下相关的程序。

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


