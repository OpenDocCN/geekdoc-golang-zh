<!--yml
category: 未分类
date: 2024-10-13 06:43:09
-->

# Append or Add one slice to another slice in Go (Golang)

> 来源：[https://golangbyexample.com/append-one-slice-another-golang/](https://golangbyexample.com/append-one-slice-another-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

It is also possible to append one slice to another slice. Below is the format for that.

```
res := append(slice1, slice2...)
```

Notice **‘…’** after the second slice.** ‘…’ **is the operator which means that the argument is a variadic parameter. Meaning that during run time slice2 will be expanded to its individual elements which are passed as multiple arguments to the append function.

## **Program**

```
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

**Output**

```
numbers=[1 2 3 4]
length=4
capacity=4
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*