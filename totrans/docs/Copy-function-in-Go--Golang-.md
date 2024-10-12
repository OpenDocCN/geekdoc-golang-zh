<!--yml
category: 未分类
date: 2024-10-13 06:18:40
-->

# Copy function in Go (Golang)

> 来源：[https://golangbyexample.com/copy-function-in-golang/](https://golangbyexample.com/copy-function-in-golang/)

go **builtin** package provides **copy** function that can be used to copy a slice. Below is the signature of this function. It takes in two slices **dst** and **src**, and copies data from **src** to **dst**. It returns the number of elements copied.

```
func copy(dst, src []Type) int
```

As the signature suggests the copy function can be used to copy slice of different types from src to dst. There are two cases to be considered while using the copy function:

*   If the length of **src** is greater than the length of **dst**, then the number of elements copied is the length of **dst**

*   If the length of **dst** is greater than length of **src,** then number of elements copied is length of **src**

Basically the number of elements copied is minimum of length of **(src, dst). **

Also to note that once the copy is done then any change in **dst** will not reflect in **src** and vice versa unless both **src** and **dst** refer to the same slice.

Simple copy example for an integer type illustrating above points

```
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

**Output**

```
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

**copy function for string**

A string in go is nothing but a sequence of bytes. Hence it is legal to copy a string to a slice of bytes.

```
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

**Output**

```
Number Of Elements Copied: 3
dst: [97 98 99]
src: abc
```

While using a copy function the source and destination can overlap too. So it is possible to copy from a slice to itself as well. In below example we copy the last 2 elements of the slice to the starting two elements in same order

```
package main

import "fmt"

func main() {
    src := []int{1, 2, 3, 4, 5}
    numberOfElementsCopied := copy(src, src[3:])

    fmt.Printf("Number Of Elements Copied: %d\n", numberOfElementsCopied)
    fmt.Printf("src: %v\n", src)
}
```

**Output**

```
Number Of Elements Copied: 2
src: [4 5 3 4 5]
```

**Conclusion**

This is all about copy function in go. Hope you have liked this article.