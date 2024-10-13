<!--yml

类别：未分类

日期：2024-10-13 06:27:03

-->

# defer在Go（Golang）中是如何工作的

> 来源：[https://golangbyexample.com/how-defer-works-golang/](https://golangbyexample.com/how-defer-works-golang/)

目录

**   [概述](#Overview "概述")

+   [单个Defer](#Single_Defer "单个Defer")

+   [特定函数中的多个Defer函数](#Multiple_Defer_function_within_a_particular_function "特定函数中的多个Defer函数")

+   [不同函数中的多个Defer函数](#Multiple_Defer_function_in_different_functions "不同函数中的多个Defer函数")*  *# **概述**

当编译器在一个函数中遇到defer语句时，它会将其推入一个列表。该列表内部实现了类似堆栈的数据结构。同一函数中遇到的所有defer语句都会被推入这个列表。当外部函数返回时，堆栈中的所有函数将从上到下依次执行，然后才能开始调用函数的执行。调用函数中也会发生相同的事情。

让我们看三个defer的例子，更好地理解defer在Go中的工作原理。

+   单个defer

+   同一函数中的多个defer

+   不同函数中的不同defer

让我们看每个示例

# **单个Defer**

```
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

```
Executed in main
In Defer
```

在上面的程序中，有一个**defer**语句调用了名为**test**的自定义函数。从输出可以看出，**test**函数在主函数的所有内容执行完后被调用，并在主函数返回之前执行。这就是原因。

```
Executed in main
```

在此之前会打印

```
In Defer
```

# **特定函数中的多个Defer函数**

如果在特定函数中有多个defer函数，则所有defer函数将按照后进先出的顺序执行，这与我们上述提到的相似。

让我们看一下这个程序。

```
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

```
3
2
1
```

在上面的程序中，我们有三个**defer**函数，每个函数打印**i**的值。变量**i**在每个defer之前递增。代码首先输出3，表示第三个defer函数首先执行。然后输出2，表示第二个defer在其后执行，最后输出1，表示第一个defer最后执行。这表明，当特定函数中有多个defer函数时，遵循“后进先出”的规则。这就是程序输出的原因。

```
3
2
1
```

# **不同函数中的多个Defer函数**

让我们理解当在不同函数中有多个defer函数时会发生什么。想象一下从**main**函数调用**f1**函数，再调用**f2**函数的情况。

**main**->**f1**->**f2**

以下是f2返回后将发生的顺序

+   如果存在**f2**中的defer函数，将执行这些defer函数。控制将返回给调用者，即函数**f1**。

+   如果在**f1**中存在defer函数，它们将被执行。控制将返回给调用者，即函数**main**。请注意，如果中间有更多函数，处理将以类似的方式向上继续执行。

+   在主函数返回后，如果主函数中存在defer函数，它将被执行。

让我们来看一个程序示例。

```
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

```
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

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
