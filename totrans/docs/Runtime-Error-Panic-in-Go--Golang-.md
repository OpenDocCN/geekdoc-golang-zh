<!--yml
category: 未分类
date: 2024-10-13 06:25:46
-->

# Runtime Error Panic in Go (Golang)

> 来源：[https://golangbyexample.com/runtime-error-panic-golang/](https://golangbyexample.com/runtime-error-panic-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Runtime error in the program can happen in the below cases. All the below cases will also create a **panic**

*   Out of bounds array access

*   Calling a function on a nil pointer

*   Sending on a closed channel

*   Incorrect type assertion

# **Example**

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

In the above program, we have a slice of length 2 and we are trying to access slice at index 2 in the **print** function. Out of bound access is not allowed and it will create panic as seen from the output. Notice that in the output there are two things

*   The error message

*   Stack trace of where the panic happened

There are many more cases in which runtime error can happen in a program. We are not going to mention all of them but you get the idea

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*