<!--yml
category: 未分类
date: 2024-10-13 06:52:25
-->

# Enum in Golang

> 来源：[https://golangbyexample.com/enum-in-golang/](https://golangbyexample.com/enum-in-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# Overview

**IOTA** provides an automated way to create an enum in Golang. Let’s see an example.

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

In above program we created a new type

```
type Size uint8
```

Then we declared some const of  type **Size**. The first constant **small** is set to iota so it will be set to zero

```
small Size = iota
```

That’s why

```
fmt.Println(small)      >> outputs 0  
fmt.Println(medium)     >> outputs 1
fmt.Println(large)      >> outputs 2
fmt.Println(extraLarge) >> outputs 3
```

Without IOTA we had to explicitly set the values of each of the enum value

```
package main

import "fmt"

type Size uint8

const (
	small      Size = 0
	medium     Size = 1
	large      Size = 2
	extraLarge Size = 3
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

We have defined a **toString** method for the **Size** type. It can be used to the print the string value of the constant of Size type.*