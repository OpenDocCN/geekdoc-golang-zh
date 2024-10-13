<!--yml

类别：未分类

日期：2024-10-13 06:18:46

-->

# Go（Golang）中的append函数

> 来源：[https://golangbyexample.com/append-function-in-golang/](https://golangbyexample.com/append-function-in-golang/)

Go **内置**包提供了一个**append**函数，可以用于在切片末尾追加。以下是此函数的签名。

```go
func append(slice []Type, elems ...Type) []Type
```

第一个参数是切片本身。第二个是可变数量的参数。

```go
elems ...Type
```

**‘…’**运算符是可变参数语法。因此，基本上**…Type**意味着append函数可以接受类型为**Type**的可变数量参数。以下是使用此函数的方法。在下面的代码中，我们将4附加到一个包含两个元素的切片中。它会在末尾追加并返回原始切片。这就是为什么我们再次在**numbers**变量中收集结果。将结果分配给其他变量也是可以的。

```go
numbers := []int{1,2}
numbers := append(numbers, 4) //Slice will become [1, 2, 3, 4]
```

追加多个元素也是可以的，因为第二个参数是可变参数。

```go
numbers := []int{1,2}
numbers := append(numbers, 3, 4, 5) //Slice will become [1, 2, 3, 4, 5]
```

此函数在后台增加切片的长度和容量。总共有两种情况。

+   当切片长度小于容量时

在这种情况下，使用append函数时，切片的长度将增加1，而容量不变。让我们看一个例子。

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

在所有情况下，容量不变，仍为5，而长度增加1。

+   当切片长度等于容量时。

在这种情况下，由于没有更多的容量，因此无法容纳新元素。所以在这种情况下，底层会分配一个容量加倍的数组。当前由切片指向的数组将被复制到该新数组。现在切片将开始指向这个新数组。因此，容量将加倍，长度将增加1。让我们看一个例子

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

请注意，上例中的容量被加倍。

也可以将一个切片附加到另一个切片。以下是该格式。

```go
res := append(slice1, slice2...)
```

请注意第二个切片后的**‘…’**。**‘…’**是运算符，表示参数是可变参数。意味着在运行时，slice2将扩展为其各个元素，并作为多个参数传递给append函数。

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

**字符串的append函数**

在Go中，字符串只是一系列字节。因此，将字符串附加到字节切片是合法的。以下是相关程序。请注意字符串末尾的**‘…’**。

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
