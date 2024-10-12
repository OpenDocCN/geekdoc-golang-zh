<!--yml
category: 未分类
date: 2024-10-13 06:27:34
-->

# Create panic by calling panic function in Go (Golang)

> 来源：[https://golangbyexample.com/create-panic-golang/](https://golangbyexample.com/create-panic-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Go provides a special function to create a panic. Below is the syntax of the function

```
func panic(v interface{})
```

This function can be called explicitly by the programmer to create a panic. It takes an empty interface as an argument. 
Some of the cases where panic function can be called explicitly by the programmer are:

*   The function expected a valid argument but instead, a nil argument was supplied. In such case, program cannot continue and it will raise a panic for a nil argument passed

*   Any other scenario in which program cannot continue.

# Example

Let’s see an example

```
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
}

func checkAndPrint(a []string, index int) {
	if index > (len(a) - 1) {
		panic("Out of bound access for slice")
	}
	fmt.Println(a[index])
}
```

**Output**

```
panic: runtime error: index out of range [2] with length 2

goroutine 1 [running]:
main.checkAndPrint(0xc00009af58, 0x2, 0x2, 0x2)
        main.go:15 +0x31
main.main()
        main.go:8 +0x7d
exit status 2
```

In the above program, we again have a function **checkAndPrint** which accepts a slice as an argument and an index. Then it checks whether the index passed is greater than the length of slice minus 1\. If it is, then it is out of bounds access for the slice so it panics. If not then it prints the value at that index. Again notice that in the output there are two things

*   The error message
*   Stack trace of where the panic happened

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*