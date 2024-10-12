<!--yml
category: 未分类
date: 2024-10-13 06:28:41
-->

# IOTA in Go (Golang)

> 来源：[https://golangbyexample.com/iota-in-golang/](https://golangbyexample.com/iota-in-golang/)

This is the  chapter 22 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Goroutines](https://golangbyexample.com/goroutines-golang/)
**Previous Tutorial** – [Interface](https://golangbyexample.com/interface-in-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [More about IOTA](#More_about_IOTA "More about IOTA")
*   [Enum in Golang](#Enum_in_Golang "Enum in Golang")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Iota is an identifier which is used with constant and which can simplify constant definitions that use auto increment numbers.  The **IOTA** keyword represent integer constant starting from zero.  So essentially it can be used to create effective constant in Go . They can also be used to create enum in Go as we will see later in this tutorial.

Auto increment constant without IOTA

```
const (
    a = 0
    b = 1
    c = 2
)
```

Auto increment constant with IOTA

```
const (
    a = iota
    b
    c
)
```

Both will set

```
a=0
b=1
c=2
```

So IOTA is

*   A counter which starts with zero
*   Increases by 1 after each line
*   Is only used with constant

IOTA starts with zero and increases by 1 after each line but there are some caveats as well. First, let’s see a simple example where iota starts with zero increases by 1 after each line

```
package main

import "fmt"

const (
    a = iota
    b
    c
)
func main() {
    fmt.Println(a)
    fmt.Println(b)
    fmt.Println(c)
}
```

**Output**

```
0
1
2
```

Iota sets the value of **a** to zero. Then on each new line and it increments the value by one. Therefore the output is 0 followed by 1 followed by 2

# **More about IOTA**

Let’s see some other points related to iota

*   Iota keyword can be used on each line as well. In that case, also iota will start from zero and increment on each new line. It will be the same as the above case

```
const (
	a = iota
	b = iota
	c = iota
)
```

will output

```
0
1
2
```

*   iota keyword can be skipped as well. In that case, also iota will start from zero and increment on each new line. It is same as above two cases

```
const (
    a = iota
    b
    c = iota
)
```

will output

```
0
1
2
```

*   There will be no increment if there is a empty line or a commented line

```
const (
	a = iota

	b
	//comment
	c
)
```

will output

```
0
1
2
```

*   Iota value will reset and again start with zero if the const keyword is used again

```
const (
	a = iota
	b
)
const (
	c = iota
)
```

will output

```
0
1
0
```

*   iota increment can be skipped using a blank identifier

```
const (
	a = iota
	_
	b
	c
)
```

will output

```
0
2
3
```

*   iota expressions – iota allows expressions which can be used to set any value for the constant

```
package main

import "fmt"

const (
	a = iota
	b = iota + 4
	c = iota * 4
)

func main() {
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)
}
```

will output

```
0
5
8
```

The first-time iota value was zero, hence the output is zero.

On the next line iota value is 1 hence the output is 1+4=5.

On the next line, iota value is 2 hence output 2*4=8

*   iota can also start from non-zero number- iota expressions can also be used to start iota from any number

```
const (
	a = iota + 10
	b
	c
)
```

will output

```
10
11
12
```

# **Enum in Golang**

**IOTA** provides an automated way to create a enum in Golang. Let’s see an example.

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

We have defined a **toString** method for the **Size** type. It can be used to the print the string value of the constant of Size type.

# **Conclusion**

This is all about IOTA and Enum in golang. Hope you have liked this article. Please share feedback/improvements/mistakes in comments

**Next Tutorial** – [Goroutines](https://golangbyexample.com/goroutines-golang/)
**Previous Tutorial** – [Interface](https://golangbyexample.com/interface-in-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*