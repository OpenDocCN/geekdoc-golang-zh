<!--yml

分类：未分类

日期：2024-10-13 06:18:28

-->

# 理解Go中的数组（Golang）– 完整指南

> 来源：[https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

这是golang综合教程系列的第17章。有关该系列其他章节，请参考此链接 – **[Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)**

**下一个教程** – **[切片](https://golangbyexample.com/slice-in-golang/)**

**前一个教程** – [结构体](https://golangbyexample.com/struct-in-golang-complete-guide/)

现在让我们查看当前的教程。以下是当前教程的目录。

目录

**[概述](#Overview "Overview")**

    +   **[定义](#Definition "Definition")**

+   **[数组声明](#Declaration_of_an_array "Declaration of an array")**

+   **[访问数组元素](#Accessing_array_elements "Accessing array elements")**

+   **[数组在go中是值](#Arrays_are_value_in_go "Arrays are value in go")**

+   **[迭代数组的不同方法](#Different_ways_of_iterating_an_array "Different ways of iterating an array")**

+   **[多维数组](#MultiDimensional_Arrays "MultiDimensional Arrays")**

+   **[结论](#Conclusion "Conclusion")** # **概述**

类似于其他编程语言，golang也有**数组**数据结构。但在go中，**数组**的行为与其他语言略有不同，我们还有一种称为切片的东西，它类似于数组的引用。在本文中，我们将仅研究数组。

## **定义**

数组是同一类型元素的连续集合。它是存储在内存中连续的元素有序序列。

这是数组声明的格式。

```go
sample := [size_of_array]{type}{a1, a2... an}
```

+   size_of_array – 数组中的元素数量

+   <type>是数组中每个元素的类型。

+   a1, a2 … an是实际元素。

在golang中，数组的大小是其类型的一部分。这意味着两个具有不同元素数量的数组是两种不同的类型，不能相互赋值。如果尝试赋值两个不同长度的数组，将会引发下面的错误。

```go
cannot use sample1 (type [1]int) as type [2]int in assignment
```

代码如下：

```go
sample1 := [1]int{1}
sample2 := [2]int{1,2}

sample2 = sample1
```

由于同样的原因，数组的长度在创建时是固定的，不能在后续更改。

# **数组声明**

数组声明中元素的数量和实际元素都是可选的。

在下面的例子中，我们看到声明数组的4种方式。

+   同时指定数组的长度和实际元素。例如：

```go
[2]int{1, 2}
```

+   仅长度 – 在这种情况下，所有实际元素都填充为该类型的默认值零。例如：

```go
[2]int{}
```

+   仅实际元素 – 在这种情况下，数组的长度将等于实际元素的数量。当不指定长度时，符号**‘…’**需要在方括号内使用，格式为**[…]**。该符号是对编译器的指令，以计算长度。

```go
[...]int{2, 3}
```

+   如果没有长度和实际元素——在这种情况下，将创建一个空数组。与上面类似，符号 **‘…’** 在这种情况下也需要使用。

```go
[...]int{}
```

让我们看一个代码示例来说明上述要点。还请记住，内置函数 **len()** 可以用来计算数组的长度。在下面的程序中，我们使用 **len()** 函数来计算数组的长度。

```go
package main

import "fmt"

func main() {
    //Both number of elements and actual elements
    sample1 := [2]int{1, 2}
    fmt.Printf("Sample1: Len: %d, %v\n", len(sample1), sample1)

    //Only actual elements
    sample2 := [...]int{2, 3}
    fmt.Printf("Sample2: Len: %d, %v\n", len(sample2), sample2)

    //Only number of elements
    sample3 := [2]int{}
    fmt.Printf("Sample3: Len: %d, %v\n", len(sample3), sample3)

    //Without both number of elements and actual elements
    sample4 := [...]int{}
    fmt.Printf("Sample4: Len: %d, %v\n", len(sample4), sample4)
}
```

**输出**

```go
Sample1: Len: 2, [1 2]
Sample2: Len: 2, [2 3]
Sample3: Len: 2, [0 0]
Sample4: Len: 0, []
```

注意在上面的示例中，对于 **sample3** 变量，实际元素填充了 int 的默认值，即 0。

如果实际指定的元素少于数组的长度，也是可以的。其余元素将用指定类型的默认值填充。请参见下面的示例。指定的数组长度为 4，而只声明了 2 个实际元素。因此，其余两个元素被赋值为 0，这是 **int** 的默认零值。

```go
package main

import "fmt"

func main() {
    sample := [4]int{5, 8}
    fmt.Printf("Sample: Len: %d, %v\n", len(sample), sample)
}
```

**输出**

```go
Sample: Len: 4, [5 8 0 0]
```

# **访问数组元素**

由于数组元素是以连续方式存储的，我们可以使用索引访问数组元素。同样，单个数组元素也可以使用索引赋值。访问越界索引将导致编译错误。请参见下面的示例以说明这些要点。第一个索引位置将是 **零**，最后一个索引将是 **(length_of_array-1)**

```go
package main

import "fmt"

func main() {
    sample := [2]string{"aa", "bb"}

    fmt.Println(sample[0])
    fmt.Println(sample[1])

    sample[0] = "xx"
    fmt.Println(sample)
    //sample[3] = "yy"
}
```

**输出**

```go
aa
bb
[xx bb]
```

取消注释下面的行

```go
sample[3] = "yy"
```

, 这将导致编译错误

```go
invalid array index 3 (out of bounds for 2-element array)
```

# **数组在 Go 中是值**

在 Go 中，数组是值类型。因此，数组变量名称并不是指向第一个元素的指针，而是表示整个数组。当

+   一个数组变量被赋值给另一个数组变量。

+   一个数组变量作为参数传递给一个函数。

让我们用另一个示例来看上述要点

```go
package main

import "fmt"

func main() {
    sample1 := [2]string{"a", "b"}
    fmt.Printf("Sample1 Before: %v\n", sample1)
    sample2 := sample1
    sample2[0] = "c"
    fmt.Printf("Sample1 After assignment: %v\n", sample1)
    fmt.Printf("Sample2: %v\n", sample2)
    test(sample1)
    fmt.Printf("Sample1 After Test Function Call: %v\n", sample1)
}
func test(sample [2]string) {
    sample[0] = "d"
    fmt.Printf("Sample in Test function: %v\n", sample)
}
```

**输出**

```go
Sample1 Before: [a b]
Sample1 After assignment: [a b]
Sample2: 
Sample in Test function: [d b]
Sample1 After Test Function Call: [a b]
```

在上述示例中，

+   我们将 **sample1** 赋值给 **sample2**，然后将 **sample2** 的 0 索引位置更改为不同的值。之后，当我们打印 **sample1** 时，会发现它没有改变。这是因为当我们将 **sample1** 赋值给 **sample2** 时，创建了一个副本，改变 **sample2** 对 **sample1** 没有任何影响。

+   我们将 **sample1** 传递给测试函数，然后在测试函数中将其在 0 索引位置的值更改。之后，当我们打印 **sample1** 时，会发现它没有改变。原因是，当 **sample1** 作为参数传递给测试函数时，会创建 **sample1** 的一个副本。

# **迭代数组的不同方法**

数组可以使用以下方式迭代：

+   使用 for 循环

+   使用 for-range 循环

让我们看一个两者的代码示例

```go
package main

import "fmt"

func main() {
    letters := [3]string{"a", "b", "c"}
    //Using for loop
    fmt.Println("Using for loop")
    len := len(letters)
    for i := 0; i < len; i++ {
        fmt.Println(letters[i])
    }
    //Using for-range operator
    fmt.Println("\nUsing for-range loop")
    for i, letter := range letters {
        fmt.Printf("%d %s\n", i, letter)
    }
}
```

**输出**

```go
Using for loop
a
b
c

Using for-range loop
0 a
1 b
2 c
```

# **多维数组**

下面是声明二维数组的格式

```go
sample := [x][y]{type}{{a11, a12 .. a1y},
                       {a21, a22 .. a2y},
                       {.. },
                       {ax1, ax2 .. axy}} 
```

其中

+   x 表示行数

+   y 表示列数

+   aij 表示位于第 i 行和第 j 列的元素

同样的思路可以扩展到三维、四维等更多维度。我们上面讨论的所有规则也适用于多维数组。

让我们看一个代码示例

```go
package main

import "fmt"

func main() {
    sample := [2][3]int{{1, 2, 3}, {4, 5, 6}}
    fmt.Println("First Run")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }

    sample[0][0] = 6
    sample[1][2] = 1
    fmt.Println("\nSecond Run")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }
}
```

**输出**

```go
First Run
1
2
3
4
5
6

Second Run
6
2
3
4
5
1
```

在上面的示例中，我们使用索引访问二维数组的元素，适用于第一维和第二维。

```go
sample[0][0] = 6
```

还要注意我们是如何遍历二维数组的。我们需要使用嵌套的 range。第一个 range 遍历数组中的数组，第二个 range 在此之后遍历单个数组。

# **结论**

这篇文章全部讲的是 Golang 中的数组。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一篇教程** – [切片](https://golangbyexample.com/slice-in-golang/)

**上一篇教程** – [结构体](https://golangbyexample.com/struct-in-golang-complete-guide/)

+   [数组](https://golangbyexample.com/tag/array/) * [完整指南](https://golangbyexample.com/tag/complete-guide/) * [Go](https://golangbyexample.com/tag/go/) * [Golang](https://golangbyexample.com/tag/golang/) *
