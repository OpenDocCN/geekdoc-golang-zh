<!--yml
category: 未分类
date: 2024-10-13 06:25:56
-->

# Panic format string in Go (Golang)

> 来源：[https://golangbyexample.com/panic-format-string-go/](https://golangbyexample.com/panic-format-string-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Below is the syntax of the panic function

```
func panic(v interface{})
```

It takes empty interface as an argument. It does not provides any why to format the error string. However there is a workaround. **Sprintf** function of **fmt** package can be used to format the error message before passing it to the panic function. Let’s see a program for that

# **Example**

```
package main

import (
	"fmt"
)

func main() {
	a := []string{"a", "b"}
	checkAndPrint(a, 2)
	fmt.Println("Exiting normally")
}

func checkAndPrint(a []string, index int) {
	if index > (len(a) - 1) {
		errorString := fmt.Sprintf("Out of bounds access for slice. Index passed: %d", index)
		panic(errorString)
	}
	fmt.Println(a[index])
}
```

**Output**

```
panic: Out of bounds access for slice. Index passed: 2

goroutine 1 [running]:
main.checkAndPrint(0xc00009af58, 0x2, 0x2, 0x2)
        main.go:17 +0x157
main.main()
        main.go:10 +0x81
exit status 2
```

In the above program we have a function **checkAndPrint** which checks and prints slice element at an index passed in the argument. If the index passed is greater than the length of the array then the program panics. Notice how we format the error string before passing it to the panic in the **checkAndPrint** function

```
errorString := fmt.Sprintf("Out of bounds access for slice. Index passed: %d", index)
```

The program also outputs the correct formatted message

```
panic: Out of bounds access for slice. Index passed: 2
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*