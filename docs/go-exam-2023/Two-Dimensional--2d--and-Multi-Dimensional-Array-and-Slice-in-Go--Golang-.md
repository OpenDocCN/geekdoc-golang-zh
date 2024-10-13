<!--yml

分类: 未分类

日期: 2024-10-13 06:18:33

-->

# Go（Golang）中的二维（2d）和多维数组及切片

> 来源：[https://golangbyexample.com/two-dimensional-array-slice-golang/](https://golangbyexample.com/two-dimensional-array-slice-golang/)

在Go语言中，多维数组和切片都是可以的。让我们详细了解它们。

目录

**   [多维数组](#Multi-Dimensional_Arrays "多维数组")

    +   [概述](#Overview "概述")

    +   [访问多维数组的元素](#Accessing_elements_of_a_multi_dimensional_array "访问多维数组的元素")

    +   [多维数组的遍历](#Traversal_of_a_multidimensional_array "多维数组的遍历")

    +   [多维数组在内存中是如何存储的](#How_multidimensional_array_is_stored_in_memory "多维数组在内存中是如何存储的")

+   [多维切片](#Multi-dimensional_Slices "多维切片")

    +   [概述](#Overview-2 "概述")

    +   [访问多维切片元素](#Accessing_Multi-Dimensional_Slice_elements "访问多维切片元素")

    +   [多维切片的遍历。](#Traversal_of_a_multi_dimensional_slice "多维切片的遍历。")

    +   [多维切片在内存中是如何存储的](#How_multidimensional_slice_is_stored_in_memory "多维切片在内存中是如何存储的")

+   [结论](#Conclusion "结论")*  *# **多维数组**

## **概述**

下面是声明多维数组的格式

```go
[len1][len2][len3]....[lenN]T{}
```

其中

+   len1、len2 .. lenN 是每个维度的长度

+   T 是数据类型

所有适用于一维数组的规则也适用于多维数组。声明时也可以指定数组元素。如果在声明时未指定数组元素，则所有数组元素都分配了**<data_type>** 的默认零值。

下面是声明一个指定数组元素的二维数组的格式。

```go
var sample = [len1][len2]T{{a11, a12 .. a1y},
                       {a21, a22 .. a2y},
                       {.. },
                       {ax1, ax2 .. axy}} 
```

其中

+   **len1** 表示行数

+   **len2** 表示列的数量

+   **aij** 表示位于第 i 行第 j 列的元素

+   **T** 是数据类型

让我们看看一个小示例，说明上述要点：

```go
package main

import "fmt"

func main() {

    sample := [2][3]int{{1, 2, 3}, {4, 5, 6}}

    fmt.Printf("Number of rows in array: %d\n", len(sample))
    fmt.Printf("Number of columns in array: %d\n", len(sample[0]))
    fmt.Printf("Total number of elements in array: %d\n", len(sample)*len(sample[0]))

    fmt.Println("Traversing Array")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }
}
```

**输出**

```go
Number of rows in array: 2
Number of columns in array: 3
Total number of elements in array: 6
Traversing Array
1
2
3
4
5
6
```

请注意在上面的程序中，我们是如何得到数组的行数、列数以及总元素数的

+   行数 = len(sample)

+   列数 = len(sample[0])

+   总元素数 = len(sample) * len(sample[0])

相同的思路可以扩展到三维、四维等等。让我们看看一个小的三维数组示例。在下面的程序中，我们创建了一个 2*2*3 的三维数组。

```go
package main

import "fmt"

func main() {
    sample := [2][2][3]int{{{1, 2, 3}, {4, 5, 6}}, {{7, 8, 9}, {10, 11, 12}}}

    fmt.Printf("Length of first dimension: %d\n", len(sample))
    fmt.Printf("Length of second dimension: %d\n", len(sample[0]))
    fmt.Printf("Length of third dimension: %d\n", len(sample[0][0]))

    fmt.Printf("Overall Dimension of the array: %d*%d*%d\n", len(sample), len(sample[0]), len(sample[0][0]))
    fmt.Printf("Total number of elements in array: %d\n", len(sample)*len(sample[0])*len(sample[0][0]))

    for _, first := range sample {
        for _, second := range first {
            for _, value := range second {
                fmt.Println(value)
            }
        }
    }
}
```

**输出**

```go
Length of first dimension: 2
Length of second dimension: 2
Length of third dimension: 3
Overall Dimension of the array: 2*2*3
Total number of elements in array: 12
1
2
3
4
5
6
7
8
9
10
11
12
```

## **访问多维数组的元素**

多维数组的元素可以通过每个维度的索引进行访问。例如，二维数组可以通过提供其行索引和列索引进行访问。一旦我们能够使用每个维度的索引访问它们，那么也可以给它分配一个新值。让我们看看一个程序。

```go
package main

import "fmt"

func main() {
    sample := [2][3]int{{1, 2, 3}, {4, 5, 6}}
    //Print array element
    fmt.Println(sample[0][0])
    fmt.Println(sample[0][1])
    fmt.Println(sample[0][2])
    fmt.Println(sample[1][0])
    fmt.Println(sample[1][1])
    fmt.Println(sample[1][2])

    //Assign new values
    sample[0][0] = 6
    sample[0][1] = 5
    sample[0][2] = 4
    sample[1][0] = 3
    sample[1][1] = 2
    sample[1][2] = 1

    fmt.Println()
    fmt.Println(sample[0][0])
    fmt.Println(sample[0][1])
    fmt.Println(sample[0][2])
    fmt.Println(sample[1][0])
    fmt.Println(sample[1][1])
    fmt.Println(sample[1][2])
}
```

**输出**

```go
1
2
3
4
5
6

6
5
4
3
2
1
```

## **遍历多维数组**

多维数组可以通过以下方式遍历：

+   for-range循环

+   for循环

让我们来看一个遍历二维数组的代码示例。

```go
package main

import "fmt"

func main() {
    sample := [2][3]int{{1, 2, 3}, {4, 5, 6}}
    fmt.Println("Using for-range")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }

    fmt.Println("\nUsing for loop")
    for i := 0; i < 2; i++ {
        for j := 0; j < 3; j++ {
            fmt.Println(sample[i][j])
        }
    }

    fmt.Println("\nUsing for loop - Second way")
    for i := 0; i < len(sample); i++ {
        for j := 0; j < len(sample[i]); j++ {
            fmt.Println(sample[i][j])
        }
    }
}
```

**输出**

```go
Using for-rage
1
2
3
4
5
6

Using for loop
1
2
3
4
5
6

Using for loop 
1
2
3
4
5
6
```

关于上述程序的一些要点

+   我们必须使用嵌套范围进行遍历，使用for-range循环。第一个范围遍历每一行。第二个范围遍历该行中的各个数组。

+   迭代使用for循环也是如此。

+   len(sample) 给出行数。

+   len(sample[i]) 给出行i中列的数量。

+   同样的思想也可以扩展到三维、四维数组元素。

## **多维数组在内存中的存储方式**

为数组分配的内存是连续的，无论数组是一维还是二维。例如，在二维数组的情况下，第二行在内存中从第一行结束的地方开始。让我们看看一个说明这一点的程序。

```go
package main

import "fmt"

func main() {
    sample := [2][3]byte{}

    fmt.Println("First row")
    fmt.Println(&sample[0][0])
    fmt.Println(&sample[0][1])
    fmt.Println(&sample[0][2])

    fmt.Println("\nSecond row")
    fmt.Println(&sample[1][0])
    fmt.Println(&sample[1][1])
    fmt.Println(&sample[1][2])
}
```

**输出**

```go
First row
0xc0000b4002
0xc0000b4003
0xc0000b4004

Second row
0xc0000b4005
0xc0000b4006
0xc0000b4007
```

注意所有地址是连续的。第二行从第一行结束的地方开始。

# **多维切片**

## **概述**

由于多维数组是数组的数组，因此多维切片也是切片的切片。要理解这一点，我们先来看切片的定义。切片指向一个底层数组，并由切片头表示。切片头是一个结构体，样子如下。

```go
type SliceHeader struct {
        Data uintptr
        Len  int
        Cap  int
}
```

切片头中的**数据**字段是指向底层数组的指针。对于一维切片，我们有如下声明。

```go
oneDSlice := make([]int, 2)
```

要声明一个二维切片，声明如下。

```go
twoDSlice := make([][]int, 2)
```

上述声明意味着我们想要创建一个**切片**，包含2个切片。仔细理解这一点。但在这里等一下，我们还没有指定第二个维度，这意味着每个内层切片的长度。在切片的情况下，每个内层切片必须像下面这样明确初始化。

```go
for i := range twoDSlice {
    twoDSlice[i] = make([]int, 3)
}
```

所以使用原始切片上的范围，我们使用make指定每个2个切片的长度。下面是另一种相同的方法，但指定了切片元素。

```go
var twoDSlice = make([][]int, 2)
twoDSlice[0] = []int{1, 2, 3}
twoDSlice[1] = []int{4, 5, 6}
```

基本上，通过上述声明，我们创建了一个2*3维的切片，这就是一个二维切片。同样的思想可以扩展到二维、三维，等等。

上述两点的完整工作示例

```go
package main

import "fmt"

func main() {
    twoDSlice1 := make([][]int, 3)
    for i := range twoDSlice1 {
        twoDSlice1[i] = make([]int, 3)
    }
    fmt.Printf("Number of rows in slice: %d\n", len(twoDSlice1))
    fmt.Printf("Number of columns in arsliceray: %d\n", len(twoDSlice1[0]))
    fmt.Printf("Total number of elements in slice: %d\n", len(twoDSlice1)*len(twoDSlice1[0]))
    fmt.Println("First Slice")
    for _, row := range twoDSlice1 {
        for _, val := range row {
            fmt.Println(val)
        }
    }
    twoDSlice2 := make([][]int, 2)
    twoDSlice2[0] = []int{1, 2, 3}
    twoDSlice2[1] = []int{4, 5, 6}
    fmt.Println()
    fmt.Printf("Number of rows in slice: %d\n", len(twoDSlice2))
    fmt.Printf("Number of columns in arsliceray: %d\n", len(twoDSlice2[0]))
    fmt.Printf("Total number of elements in slice: %d\n", len(twoDSlice2)*len(twoDSlice2[0]))
    fmt.Println("Second Slice")
    for _, row := range twoDSlice2 {
        for _, val := range row {
            fmt.Println(val)
        }
    }
}
```

**输出**

```go
Number of rows in slice: 2
Number of columns in arsliceray: 3
Total number of elements in slice: 6
First Slice
0
0
0
0
0
0

Number of rows in slice: 2
Number of columns in arsliceray: 3
Total number of elements in slice: 6
Second Slice
1
2
3
4
5
6
```

我们上面提到，我们正在创建一个二维切片，维度为2*3。也就是说，您可能会想到的问题是，内层切片是否可以有不同的长度。是的，这是可能的。与具有相同长度内层数组的数组不同，在切片的情况下，由于我们分别初始化每个内层切片，因此内层切片可以具有不同的长度。

让我们来看一个示例。

```go
package main

import "fmt"

func main() {
    twoDSlice := make([][]int, 2)
    twoDSlice[0] = []int{1, 2, 3}
    twoDSlice[1] = []int{4, 5}

    fmt.Printf("Number of rows in slice: %d\n", len(twoDSlice))
    fmt.Printf("Len of first row: %d\n", len(twoDSlice[0]))
    fmt.Printf("Len of second row: %d\n", len(twoDSlice[1]))
    fmt.Println("Traversing slice")
    for _, row := range twoDSlice {
        for _, val := range row {
            fmt.Println(val)
        }
    }
}
```

**输出**

```go
Number of rows in slice: 2
Len of first row: 3
Len of second row: 2
Traversing slice
1
2
3
4
5
```

让我们看看一个三维切片的小示例。在下面的程序中，我们创建一个维度为2*2*3的切片。

```go
package main

import "fmt"

func main() {
    sample := make([][][]int, 2)
    for i := range sample {
        sample[i] = make([][]int, 2)
        for j := range sample[i] {
            sample[i][j] = make([]int, 3)
        }
    }

    fmt.Printf("Length of first dimension: %d\n", len(sample))
    fmt.Printf("Length of second dimension: %d\n", len(sample[0]))
    fmt.Printf("Length of third dimension: %d\n", len(sample[0][0]))
    fmt.Printf("Overall Dimension of the slice: %d*%d*%d\n", len(sample), len(sample[0]), len(sample[0][0]))
    fmt.Printf("Total number of elements in slice: %d\n", len(sample)*len(sample[0])*len(sample[0][0]))
    for _, first := range sample {
        for _, second := range first {
            for _, value := range second {
                fmt.Println(value)
            }
        }
    }
}
```

**输出**

```go
Length of first dimension: 2
Length of second dimension: 2
Length of third dimension: 3
Overall Dimension of the slice: 2*2*3
Total number of elements in slice: 12
0
0
0
0
0
0
0
0
0
0
0
```

## **访问多维切片元素**

访问切片元素与访问数组元素是相同的。让我们来看一个示例。

```go
package main

import "fmt"

func main() {
    sample := make([][]int, 2)
    sample[0] = []int{1, 2, 3}
    sample[1] = []int{4, 5, 6}

    //Print array element
    fmt.Println(sample[0][0])
    fmt.Println(sample[0][1])
    fmt.Println(sample[0][2])
    fmt.Println(sample[1][0])
    fmt.Println(sample[1][1])
    fmt.Println(sample[1][2])

    //Assign new values
    sample[0][0] = 6
    sample[0][1] = 5
    sample[0][2] = 4
    sample[1][0] = 3
    sample[1][1] = 2
    sample[1][2] = 1

    fmt.Println()
    fmt.Println(sample[0][0])
    fmt.Println(sample[0][1])
    fmt.Println(sample[0][2])
    fmt.Println(sample[1][0])
    fmt.Println(sample[1][1])
    fmt.Println(sample[1][2])
}
```

**输出**

```go
1
2
3
4
5
6

6
5
4
3
2
1
```

## **多维切片的遍历。**

遍历多维切片与遍历多维数组是相同的。可以使用以下方式遍历多维切片：

+   for-range 循环

+   for 循环

让我们来看一个示例：

```go
package main

import "fmt"

func main() {
    sample := make([][]int, 2)
    sample[0] = []int{1, 2, 3}
    sample[1] = []int{4, 5, 6}
    fmt.Println("Using for-range")
    for _, row := range sample {
        for _, val := range row {
            fmt.Println(val)
        }
    }

    fmt.Println("\nUsing for loop - Second way")
    for i := 0; i < len(sample); i++ {
        for j := 0; j < len(sample[i]); j++ {
            fmt.Println(sample[i][j])
        }
    }
}
```

**输出**

## **多维切片在内存中的存储方式**

由于在切片的情况下，每个内层切片是分别初始化的，因此内层切片在内存中可能不是彼此连续的。尽管每个内层切片内的每个元素将在连续的位置上。让我们看一个程序来说明这一点。

```go
package main

import "fmt"

func main() {
    sample := make([][]byte, 2)
    sample[0] = make([]byte, 3)

    //testVariable := "s"
    //fmt.Println(testVariable)

    sample[1] = make([]byte, 3)

    fmt.Println("First row")
    fmt.Println(&sample[0][0])
    fmt.Println(&sample[0][1])
    fmt.Println(&sample[0][2])

    fmt.Println("\nSecond row")
    fmt.Println(&sample[1][0])
    fmt.Println(&sample[1][1])
    fmt.Println(&sample[1][2])
}
```

**输出**

```go
First row
0xc000018072
0xc000018073
0xc000018074

Second row
0xc000018080
0xc000018081
0xc000018082
```

**请注意：** 在上面的程序中有一个警告。由于第二个内层切片是在第一个内层切片之后初始化的，因此它们被分配的地址有可能是连续的。这可能发生，但并不总是如此。取消注释测试变量的那一行，然后两个内层切片将不会被分配连续的地址。在数组的情况下，所有内层数组的存储位置总是连续的。

# **结论**

这就是关于Golang中的多维数组和切片的全部内容。希望您喜欢这篇文章。请在评论中分享您的反馈。

+   [二维切片](https://golangbyexample.com/tag/2d/)*   [三维切片](https://golangbyexample.com/tag/3d/)*   [数组](https://golangbyexample.com/tag/array/)*   [Go语言](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*   [切片](https://golangbyexample.com/tag/slice/)*
