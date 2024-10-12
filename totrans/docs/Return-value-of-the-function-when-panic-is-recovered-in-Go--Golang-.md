<!--yml
category: 未分类
date: 2024-10-13 06:26:12
-->

# Return value of the function when panic is recovered in Go (Golang)

> 来源：[https://golangbyexample.com/return-value-function-panic-recover-go/](https://golangbyexample.com/return-value-function-panic-recover-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

When the panic is recovered then the return value of a panicking function will be the default value of the return types of  the panicking function

# **Program**

Let’s see a program for it

```
package main
import (
    "fmt"
)
func main() {
    a := []int{5, 6}
    val, err := checkAndGet(a, 2)
    fmt.Printf("Val: %d\n", val)
    fmt.Println("Error: ", err)
}
func checkAndGet(a []int, index int) (int, error) {
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    return a[index], nil
}
func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
    }
}
```

**Output**

```
Recovering from panic: Out of bound access for slice
Val: 0
Error: 
```

In the above program we have **checkAndGet** function which gets the value at a particular index in int slice. If the index passed to this function is greater than (length of slice-1), then it raises a panic. There is also a **handleOutOfBounds** function which is used to recover from the panic.  So we pass index 2 to the **checkAndGet** function and it raises the panic which is recovered in the **handleOutOfBounds** function. That is why we first get this output

```
Recovering from panic: Out of bound access for slice
```

Notice in main function that we recollect the return value from the **checkAndGet** like this

```
val, err := checkAndGet(a, 2)
```

**checkAndGet** has two return values

*   int
*   error

Since **checkAndGet** creates panic which is recovered in the handleOutOfBounds function therefore the return value of the **checkAndGet** will be the default value of its types.

Therefore

```
fmt.Printf("Val: %d\n", val)
```

outputs

```
Val: 0
```

because zero is the default value of **int** type.

And

```
fmt.Println("Error: ", err)
```

outputs

```
Error: 
```

because nil is the default value of **error** type.

If you don’t want to return default zero value of types then named return value can be used. Let’s see a program for that.

```
package main
import (
    "fmt"
)
func main() {
    a := []int{5, 6}
    val, err := checkAndGet(a, 2)
    fmt.Printf("Val: %d\n", val)
    fmt.Println("Error: ", err)
}
func checkAndGet(a []int, index int) (value int, err error) {
    value = 10
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    value = a[index]
    return value, nil
}
func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
    }
}
```

**Output**

```
Recovering from panic: Out of bound access for slice
Val: 10
Error: 
```

This program is same as previous program, the only difference being that we are using named return value in the **checkAndGet** function.

```
func checkAndGet(a []int, index int) (value int, err error)
```

We set the named return value to 10 in **checkAndGet** function

```
value = 10
```

That is why we get below output in this program as panic is created and it is recovered

```
Recovering from panic: Out of bound access for slice
Val: 10
Error: 
```

Also note that If panic would not have created in the program then it would have output the correct value at index.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*