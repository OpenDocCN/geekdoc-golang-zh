<!--yml

类别：未分类

日期：2024-10-13 06:18:50

-->

# 在Go语言中复制数组或切片

> 来源：[https://golangbyexample.com/copy-an-array-or-slice-golang/](https://golangbyexample.com/copy-an-array-or-slice-golang/)

目录

**   [概述](#Overview "概述")

+   [复制一个数组](#Copy_an_array "复制一个数组")

+   [复制一个切片](#Copy_a_slice "复制一个切片")*  *# **概述**

在Go语言中，数组是值类型，而切片是引用类型。因此，数组或切片如何复制到另一个数组或切片的方式是不同的。

# **复制一个数组**

如上所述，数组在Go语言中是值类型。因此，数组变量名不是指向第一个元素的指针，而是表示整个数组。当

+   一个数组变量被赋值给另一个数组变量。

+   一个数组变量作为参数传递给一个函数。

我们用一个例子来说明上述要点

```go
package main

import "fmt"

func main() {
	sample1 := [2]string{"a", "b"}
	fmt.Printf("Sample1 Before: %v\n", sample1)
	sample2 := sample1
	sample2[1] = "c"
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
Sample2: [a c]
Sample in Test function: [d b]
Sample1 After Test Function Call: [a b]
```

在上述例子中，

+   我们将**sample1**赋值给**sample2**，然后在**sample2**的0索引处改变为不同的值。之后，当我们打印**sample1**时，发现它没有改变。这是因为当我们将**sample1**赋值给**sample2**时，会创建一个副本，改变**sample2**不会影响**sample1**。

+   我们将**sample1**传递给测试函数，然后在测试函数的0索引处再次改变其值。之后，当我们打印**sample1**时，发现它没有改变。原因相同，当**sample1**作为参数传递给测试函数时，会创建**sample1**的一个副本。

# **复制一个切片**

Go的**builtin**包提供了一个**copy**函数，可以用来复制切片。下面是这个函数的签名。它返回复制的元素数量。

```go
func copy(dst, src []Type) int
```

在使用copy函数时，有两种情况需要考虑：

+   如果**src**的长度大于**dst**的长度，则复制的元素数量为**dst**的长度

+   如果**dst**的长度大于**src**的长度，则复制的元素数量为**src**的长度

基本上，复制的元素数量是**(src, dst)**长度的最小值。

还要注意，一旦复制完成，**dst**中的任何更改将不会反映在**src**中，反之亦然。

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

    //After changing numbers2
    dst[0] = 10
    fmt.Println("\nAfter changing dst")
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
```*
