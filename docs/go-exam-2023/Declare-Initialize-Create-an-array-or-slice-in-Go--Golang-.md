<!--yml

类别：未分类

日期：2024-10-13 06:20:36

-->

# 在 Go（Golang）中声明/初始化/创建数组或切片

> 来源：[`golangbyexample.com/declare-initialize-create-array-slice-golang/`](https://golangbyexample.com/declare-initialize-create-array-slice-golang/)

目录

**概述**

+   创建切片

    +   [使用 []<类型>{} 格式](#Using_the_format "使用 []<类型>{} 格式")

    +   从另一个切片或数组创建切片

        +   从数组创建切片

        +   从切片创建切片

    +   使用 make 函数

    +   使用新函数

+   创建数组 *# **概述**

与其他编程语言类似，Golang 也有数组数据结构。但在 Go 中，数组的行为与其他语言略有不同，我们还有一种叫切片的结构，它类似于数组的引用。切片比数组更强大且更方便使用。事实上，切片在其他编程语言中更类似于数组。

在这篇文章中，我们将学习如何

+   创建切片实例

+   创建数组实例

# **创建切片**

创建切片的方式有四种

+   使用 []<类型>{} 格式

+   从另一个切片或数组创建切片

+   使用 make

+   使用新方法

让我们逐一查看上述每种方法。

## **使用 []<类型>{} 格式**

声明切片的最常见方式是这样的

```go
s := []int
```

它声明了一个长度为 0、容量为 0 的空切片。我们也可以在声明时初始化切片。

```go
s := []int{1,2}
```

它声明了一个长度为 2、容量也为 2 的整数切片。容量等于实际指定的切片元素数量。我们还有两个由 Go 提供的库函数可以用来获取切片的 **长度** 和 **容量**。

+   **len()** 函数 – 用于切片的长度

+   **cap()** 函数 – 用于切片的容量

让我们看看一个小程序，展示以上几点。

```go
package main

import "fmt"

func main() {
    sample := []int{}
    fmt.Println(len(sample))
    fmt.Println(cap(sample))
    fmt.Println(sample)

    letters := []string{"a", "b", "c"}
    fmt.Println(len(letters))
    fmt.Println(cap(letters))
    fmt.Println(letters)
}
```

**输出**

```go
0
0
[]

3
3
[a b c]
```

当实际元素未指定时，切片的长度和容量均为零。当指定实际元素时，长度和容量等于指定的实际元素数量。

## **从另一个切片或数组创建切片**

切片可以通过重新切片现有切片或数组来创建。

### **从数组创建切片**

通过重新切片现有数组创建新切片的格式为

```go
[n]sample[start:end]
```

上述操作将返回一个新切片，从数组的索引 **start** 开始到索引 **end-1**。因此，索引 **end** 的元素不包含在新创建的切片中。在重新切片时，起始和结束索引都是可选的。

+   起始索引的默认值为零

+   结束索引的默认值是数组的长度。

让我们看看一个例子。

```go
package main

import "fmt"

func main() {
    numbers := [5]int{1, 2, 3, 4, 5}

    //Both start and end
    num1 := numbers[2:4]
    fmt.Println("Both start and end")
    fmt.Printf("num1=%v\n", num1)
    fmt.Printf("length=%d\n", len(num1))
    fmt.Printf("capacity=%d\n", cap(num1))

    //Only start
    num2 := numbers[2:]
    fmt.Println("\nOnly start")
    fmt.Printf("num1=%v\n", num2)
    fmt.Printf("length=%d\n", len(num2))
    fmt.Printf("capacity=%d\n", cap(num2))

    //Only end
    num3 := numbers[:3]
    fmt.Println("\nOnly end")
    fmt.Printf("num1=%v\n", num3)
    fmt.Printf("length=%d\n", len(num3))
    fmt.Printf("capacity=%d\n", cap(num3))

    //None
    num4 := numbers[:]
    fmt.Println("\nOnly end")
    fmt.Printf("num1=%v\n", num4)
    fmt.Printf("length=%d\n", len(num4))
    fmt.Printf("capacity=%d\n", cap(num4))
}
```

**输出**

```go
Both start and end
num1=[3 4]
length=2
capacity=3

Only start
num1=[3 4 5]
length=3
capacity=3

Only end
num1=[1 2 3]
length=3
capacity=5

Only end
num1=[1 2 3 4 5]
length=5
capacity=5
```

请注意上面的例子中：

+   新创建切片的长度 = (**end**–**start**)

+   新创建切片的容量 = (**length_of_array**–**start**)

**num1**切片的样子如下。

![](img/33586fc71a4b7b6f0294dc7dc24f41df.png)

新创建的切片仍然引用原始数组。要检查这一点，可以在数组的任意索引处更改元素，然后重新打印切片。

```go
numbers[3] = 8
fmt.Printf("num1=%v\n", num2)
fmt.Printf("num3=%v\n", num3)
fmt.Printf("num4=%v\n", num4)
```

输出如下：

```go
num1=[3 8 5]
num3=[1 2 3 8]
num4=[1 2 3 8 5]
```

这证明每个新切片仍然引用原始数组。

### **从切片创建切片**

我们关于从数组重新切片的讨论同样适用于这里。请参见下面的例子以说明同样的内容。

```go
package main

import "fmt"

func main() {
    numbers := []int{1, 2, 3, 4, 5}

    //Both start and end
    num1 := numbers[2:4]
    fmt.Println("Both start and end")
    fmt.Printf("num1=%v\n", num1)
    fmt.Printf("length=%d\n", len(num1))
    fmt.Printf("capacity=%d\n", cap(num1))

    //Only start
    num2 := numbers[2:]
    fmt.Println("\nOnly start")
    fmt.Printf("num1=%v\n", num2)
    fmt.Printf("length=%d\n", len(num2))
    fmt.Printf("capacity=%d\n", cap(num2))

    //Only end
    num3 := numbers[:3]
    fmt.Println("\nOnly end")
    fmt.Printf("num1=%v\n", num3)
    fmt.Printf("length=%d\n", len(num3))
    fmt.Printf("capacity=%d\n", cap(num3))

    //None
    num4 := numbers[:]
    fmt.Println("\nOnly end")
    fmt.Printf("num1=%v\n", num4)
    fmt.Printf("length=%d\n", len(num4))
    fmt.Printf("capacity=%d\n", cap(num4))
}
```

**输出**

```go
Both start and end
num1=[3 4]
length=2
capacity=3

Only start
num1=[3 4 5]
length=3
capacity=3

Only end
num1=[1 2 3]
length=3
capacity=5

Only end
num1=[1 2 3 4 5]
length=5
capacity=5
```

在这里，新创建的切片也引用与原始切片所引用的相同基础数组。要检查这一点，可以在原始切片的任意索引处更改元素，然后重新打印所有新创建的切片。

```go
numbers[3] = 8
fmt.Printf("num1=%v\n", num2)
fmt.Printf("num3=%v\n", num3)
fmt.Printf("num4=%v\n", num4)
```

## **使用 make 函数**

**make**是 Go 提供的一个内置函数，也可以用来创建切片。以下是 make 函数的签名。

```go
func make([]{type}, length, capacity int) []{type}
```

创建切片时，`make`函数的容量是一个可选参数。当省略容量时，切片的容量等于指定的长度。使用`make`函数时，Go 背后会分配一个与容量相等的数组。分配的数组的所有元素都初始化为该类型的默认零值。让我们看看一个说明这一点的程序。

```go
package main

import "fmt"

func main() {
    numbers := make([]int, 3, 5)
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))

    //With capacity ommited
    numbers = make([]int, 3)
    fmt.Println("\nCapacity Ommited")
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**输出**

```go
numbers=[0 0 0]
length=3
capacity=5

With Capacity Ommited
numbers=[0 0 0]
length=3
capacity=3
```

## **使用 new 函数**

**new**是 Go 提供的一个内置函数，也可以用来创建切片。这种创建切片的方式并不常用，因为**make**在功能上更加灵活。一般情况下不常使用，并且使用**new**函数会返回指向 nil 切片的指针。让我们看一个例子。在下面的例子中，我们使用解引用操作符**‘*’**，因为**new**函数返回指向 nil 切片的指针。

```go
package main

import "fmt"

func main() {
    numbers := new([]int)
    fmt.Printf("numbers=%v\n", *numbers)
    fmt.Printf("length=%d\n", len(*numbers))
    fmt.Printf("capacity=%d\n", cap(*numbers))
}
```

**输出**

```go
numbers=[]
length=0
capacity=0
```

# **创建一个数组**

数组声明中的元素数量和实际元素都是可选的。

在下面的例子中，我们看到创建数组的 4 种方式。

+   同时指定数组的长度和实际元素。例如：

```go
[2]int{1, 2}
```

+   仅长度——在这种情况下，所有实际元素都填充为该类型的默认值零。例如：

```go
[2]int{}
```

+   仅实际元素——在这种情况下，数组的长度将等于实际元素的数量。符号**‘…’**在不指定长度时需要在方括号内使用，例如[…]。该符号是指令，指示编译器计算长度。

```go
[...]int{2, 3}
```

+   没有长度和实际元素——在这种情况下，将创建一个空数组。与上述相似，符号**‘…’**在这种情况下也需要使用。

```go
[...]int{}
```

让我们看一个代码示例来说明上述要点。还请记住，可以使用内置函数**len()**来计算数组的长度。在下面的程序中，我们使用**len()**函数来计算数组的长度。

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

请注意，上述示例中，对于**sample3**变量，实际元素用默认值 0 填充。

如果指定的实际元素少于数组的长度也是可以的。其余元素将用指定类型的默认值填充。请参见下面的示例。指定的数组长度为 4，而只有 2 个实际元素被声明。因此，剩余的两个元素被赋值为 0，这是**int**的默认零值。

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

+   [Go](https://golangbyexample.com/tag/go/)*
