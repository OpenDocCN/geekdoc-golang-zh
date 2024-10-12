<!--yml
category: 未分类
date: 2024-10-13 06:26:32
-->

# How to create panic in Go (Golang)

> 来源：[https://golangbyexample.com/how-create-panic-golang/](https://golangbyexample.com/how-create-panic-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Runtime Error Panic](#Runtime_Error_Panic "Runtime Error Panic")
*   [Calling the panic function explicitly](#Calling_the_panic_function_explicitly "Calling the panic function explicitly")*  *# **Overview**

Panic is similar to the exception in golang. Panic is meant to exit from a program in abnormal conditions. Panic can occur in a program in two ways

*   Runtime error in the program

*   By calling the panic function explicitly. This can be called by the programmer when the program cannot continue and it has to exit

Go provides a special function to create a panic. Below is the syntax of the function

```
func panic(v interface{})
```

This function can be called explicitly by the programmer to create a panic. It takes an empty interface as an argument.  When a panic happens in a  program it outputs two things

*   The error message that is passed to the panic function as an argument

*   Stack trace of where the panic happened

# **Runtime Error Panic**

Runtime error in the program can happen in below cases

*   Out of bounds array access
*   Calling a function on a nil pointer
*   Sending on a closed channel
*   Incorrect type assertion

Let’s see an example of runtime error caused by out of bounds array access.

```
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	print(a, 2)
}

func print(a []string, index int) {
	fmt.Println(a[index])
}
```

**Output**

```
panic: runtime error: index out of range [2] with length 2

goroutine 1 [running]:
main.checkAndPrint(...)
        main.go:12
main.main()
        /main.go:8 +0x1b
exit status 2
```

In the above program we have a slice of length 2 and we are trying to access slice at index 3 in the **print** function. Out of bound access is not allowed and it will create panic as seen from the output. Notice that in the output there are two things

*   The error message
*   Stack trace of where the panic happened

There are many more cases in which runtime error can happen in a program. We are not going to mention all of them but you get the idea

# **Calling the panic function explicitly**

Some of the cases where the panic function can be called explicitly by the programmer are:

*   The function expected a valid argument but instead, a nil argument was supplied. In such case, the program cannot continue and it will raise a panic for a nil argument passed

*   Any other scenario in which program cannot continue.

Let’s see an example.

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