<!--yml
category: 未分类
date: 2024-10-13 06:52:30
-->

# Convert an IOTA or Enum to a string in Go (Golang)

> 来源：[https://golangbyexample.com/convert-an-iota-or-enum-to-a-string-in-go-golang/](https://golangbyexample.com/convert-an-iota-or-enum-to-a-string-in-go-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# Overview

Enum in Golang can be created by using IOTA. Please refer to this post to learn more about IOTA

> [IOTA in Go (Golang)](https://golangbyexample.com/iota-in-golang/)

[https://golangbyexample.com/iota-in-golang/embed/#?secret=0FEJ5OZNxf#?secret=4y3pVxudgT](https://golangbyexample.com/iota-in-golang/embed/#?secret=0FEJ5OZNxf#?secret=4y3pVxudgT)

In this post, we will see how to convert an IOTA or enum to a string value. By default when you will print an IOTA or enum value it will print the integer part of it. See this example. Later we will see an example of we can define a custom **toString** method to print the string value of IOTA or enum

# Example

```
package main

import "fmt"

type Size uint8

const (
	small Size = iota
	medium
	large
	extraLarge
)

func main() {
	fmt.Println(small)
	fmt.Println(medium)
	fmt.Println(large)
	fmt.Println(extraLarge)
}
```

**Output**

```
0
1
2
3
```

We can also define a method **toString** on Size type to print the exact value of enum . See below program

```
package main
import "fmt"
type Size int
const (
    small = Size(iota)
    medium
    large
    extraLarge
)
func main() {
    var m Size = 1
    m.toString()
}
func (s Size) toString() {
    switch s {
    case small:
        fmt.Println("Small")
    case medium:
        fmt.Println("Medium")
    case large:
        fmt.Println("Large")
    case extraLarge:
        fmt.Println("Extra Large")
    default:
        fmt.Println("Invalid Size entry")
    }
}
```

**Output**

```
medium
```

We will now define a **toString** method for the **Size** type. It can be used to the print the string value of the constant of Size type.*