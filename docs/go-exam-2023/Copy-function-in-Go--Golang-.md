<!--yml

类别：未分类

日期：2024-10-13 06:18:40

-->

# Go 中的复制函数（Golang）

> 来源：[https://golangbyexample.com/copy-function-in-golang/](https://golangbyexample.com/copy-function-in-golang/)

Go 的 **builtin** 包提供了 **copy** 函数，可以用于复制切片。以下是此函数的签名。它接收两个切片 **dst** 和 **src**，并将数据从 **src** 复制到 **dst**。它返回复制的元素数量。

```go
func copy(dst, src []Type) int
```

根据签名，复制函数可以用于从 src 复制不同类型的切片到 dst。在使用复制函数时，有两种情况需要考虑：

+   如果 **src** 的长度大于 **dst** 的长度，则复制的元素数量为 **dst** 的长度。

+   如果 **dst** 的长度大于 **src** 的长度，则复制的元素数量为 **src** 的长度。

基本上，复制的元素数量为 **(src, dst)** 长度的最小值。

还要注意，一旦复制完成，**dst** 中的任何更改将不会反映在 **src** 中，反之亦然，除非 **src** 和 **dst** 引用同一个切片。

一个简单的整数类型复制示例，说明上述要点。

```go
package main

import "fmt"

func main() {
    src := []int{1, 2, 3, 4, 5}
    dst := make([]int, 5)
    numberOfElementsCopied := copy(dst, src)

    fmt.Printf("Number Of Elements Copied: %d\n", numberOfElementsCopied)
    fmt.Printf("dst: %v\n", dst)
    fmt.Printf("src: %v\n", src)

    //After changing dst
    dst[0] = 10
    fmt.Println("\nAfter changing dst")
    fmt.Printf("dst: %v\n", dst)
    fmt.Printf("src: %v\n", src)

    //Length of destination is less than length of source
    dst = make([]int, 4)
    numberOfElementsCopied = copy(dst, src)
    fmt.Println("\nLength of dst less than src")
    fmt.Printf("Number Of Elements Copied: %d\n", numberOfElementsCopied)
    fmt.Printf("dst: %v\n", dst)
    fmt.Printf("src: %v\n", src)

    //Length of destination is greater than length of source
    dst = make([]int, 6)
    numberOfElementsCopied = copy(dst, src)
    fmt.Println("\nLength of dst less than src")
    fmt.Printf("Number Of Elements Copied: %d\n", numberOfElementsCopied)
    fmt.Printf("dst: %v\n", dst)
    fmt.Printf("src: %v\n", src)
}
```

**输出**

```go
Number Of Elements Copied: 5
dst: [1 2 3 4 5]
src: [1 2 3 4 5]

After changing dst
dst: [10 2 3 4 5]
src: [1 2 3 4 5]

Length of dst less than src
Number Of Elements Copied: 4
dst: [1 2 3 4]
src: [1 2 3 4 5]

Length of dst less than src
Number Of Elements Copied: 5
dst: [1 2 3 4 5 0]
src: [1 2 3 4 5]
```

**字符串的复制函数**

在 Go 中，字符串只是一系列字节。因此，将字符串复制到字节切片是合法的。

```go
package main

import "fmt"

func main() {
    src := "abc"
    dst := make([]byte, 3)

    numberOfElementsCopied := copy(dst, src)
    fmt.Printf("Number Of Elements Copied: %d\n", numberOfElementsCopied)
    fmt.Printf("dst: %v\n", dst)
    fmt.Printf("src: %v\n", src)\
}
```

**输出**

```go
Number Of Elements Copied: 3
dst: [97 98 99]
src: abc
```

在使用复制函数时，源和目标也可以重叠。因此，可以从一个切片复制到自身。以下示例中，我们将切片的最后两个元素复制到同一顺序的前两个元素中。

```go
package main

import "fmt"

func main() {
    src := []int{1, 2, 3, 4, 5}
    numberOfElementsCopied := copy(src, src[3:])

    fmt.Printf("Number Of Elements Copied: %d\n", numberOfElementsCopied)
    fmt.Printf("src: %v\n", src)
}
```

**输出**

```go
Number Of Elements Copied: 2
src: [4 5 3 4 5]
```

**结论**

这就是 Go 中的复制函数。希望你喜欢这篇文章。
