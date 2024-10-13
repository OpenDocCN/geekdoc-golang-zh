<!--yml

分类：未分类

日期：2024-10-13 06:39:40

-->

# 在 Go（Golang）中追加或添加到切片或数组。

> 来源：[`golangbyexample.com/append-slice-array-golang/`](https://golangbyexample.com/append-slice-array-golang/)

目录

**概述**

+   append() 函数 函数")

    +   当切片长度小于容量时

    +   当切片长度等于容量时

+   append() 函数用于字符串 函数用于字符串")

## **概述**

在 golang 中，**数组**的大小是其类型的一部分。这就是为什么数组的长度在创建时是固定的，之后不能更改。这就是**切片**发挥作用的地方。切片比数组更强大和方便使用。实际上，切片更类似于其他编程语言中的数组。在本教程中，我们将探讨如何向切片添加或追加内容。

切片在内部由三个部分表示。

+   指向底层数组的指针。

+   底层数组的当前长度

+   总容量是底层数组可以扩展的最大容量。

在此处了解更多关于切片的信息 – [`golangbyexample.com/slice-in-golang/`](https://golangbyexample.com/slice-in-golang/)

## **append() 函数**

go **builtin**包提供了一个**append**函数，可以在切片末尾追加或添加。以下是此函数的签名。

```go
func append(slice []Type, elems ...Type) []Type
```

第一个参数是切片本身。第二个参数是可变数量的参数。

```go
elems ...Type
```

**‘…’**运算符是可变参数语法。因此，基本上**…Type**表示追加函数可以接受类型为**Type**的可变数量的参数。以下是使用此函数的方法。在下面的代码中，我们正在向一个有两个元素的切片追加 4。它会在末尾追加并返回原始切片。这就是为什么我们再次将结果收集到**numbers**变量中的原因。将结果赋值给其他变量也是可以的。

```go
numbers := []int{1,2}
numbers := append(numbers, 4) //Slice will become [1, 2, 3, 4]
```

追加任意数量的元素也是可以的，因为第二个参数是可变参数。

```go
numbers := []int{1,2}
numbers := append(numbers, 3, 4, 5) //Slice will become [1, 2, 3, 4, 5]
```

该函数在后台增加切片的长度和容量。有两种情况。

+   当切片长度小于容量时

+   当切片长度等于容量时

### **当切片长度小于容量时**

在这种情况下，通过使用追加函数，切片的长度将增加 1，而容量不会发生变化。让我们看一个例子。

```go
package main

import "fmt"

func main() {
    numbers := make([]int, 3, 5)
    numbers[0] = 1
    numbers[1] = 2
    numbers[2] = 3
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))

    //Append number 4
    numbers = append(numbers, 4)
    fmt.Println("\nAppend Number 4")
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))

    //Append number 5
    numbers = append(numbers, 4)
    fmt.Println("\nAppend Number 5")
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**输出**

```go
numbers=[1 2 3]
length=3
capacity=5

Append Number 4
numbers=[1 2 3 4]
length=4
capacity=5

Append Number 5
numbers=[1 2 3 4 4]
length=5
capacity=5
```

在所有情况下，容量不会改变，始终是 5，而长度增加 1。

### **当切片长度等于容量时**

在这种情况下，由于没有更多的容量，所以无法容纳新的元素。因此，在后台，将分配一个容量为两倍的数组。切片指向的当前数组将被复制到那个新数组。现在切片将开始指向这个新数组。因此，容量将翻倍，长度将增加 1。让我们看一个例子。

```go
package main

import "fmt"

func main() {
    numbers := make([]int, 3, 3)
    numbers[0] = 1
    numbers[1] = 2
    numbers[2] = 3

    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))

    //Append number 4
    numbers = append(numbers, 4)
    fmt.Println("\nAppend Number 4")
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**输出**

```go
numbers=[1 2 3]
length=3
capacity=3

Append Number 4
numbers=[1 2 3 4]
length=4
capacity=6
```

注意上面的例子中容量翻倍。

也可以将一个切片附加到另一个切片。下面是该格式。

```go
res := append(slice1, slice2...)
```

注意第二个切片后面的**‘…’**。**‘…’**是表示参数为可变参数的操作符。这意味着在运行时，slice2 将扩展为其单独元素，并作为多个参数传递给 append 函数。

```go
package main

import "fmt"

func main() {
    numbers1 := []int{1, 2}
    numbers2 := []int{3, 4}
    numbers := append(numbers1, numbers2...)
    fmt.Printf("numbers=%v\n", numbers)
    fmt.Printf("length=%d\n", len(numbers))
    fmt.Printf("capacity=%d\n", cap(numbers))
}
```

**输出**

```go
numbers=[1 2 3 4]
length=4
capacity=4
```

## **append()函数用于字符串**

Go 中的字符串只是字节序列。因此，将字符串附加到字节切片是合法的。下面是该程序。注意字符串末尾的**‘…’**

```go
package main

import "fmt"

func main() {
    sample := "Hello"
    suffix := "World"

    result := append([]byte(sample), suffix...)
    fmt.Printf("sample: %s\n", string(result))
}
```

**输出**

```go
sample: HelloWorld
```

另外，查看我们的 Golang 进阶教程系列 – [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)


