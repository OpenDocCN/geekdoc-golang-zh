<!--yml
category: 未分类
date: 2024-10-13 06:34:39
-->

# How to call a function from another package in Go (Golang)

> 来源：[https://golangbyexample.com/functoin-different-package-go/](https://golangbyexample.com/functoin-different-package-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The function in another package must start with a capital letter so that it is public outside its package. If the function name starts with lowercase then it will not be visible outside its package. 
To use a function outside its package we need to import the package first which contains that function. 

## **Program**

Here is the code for the same

**go.mod**

```
module sample.com/learn
go 1.16
```

**hello/hello.go**

```
package hello

import "fmt"

func SayHello() {
	fmt.Println("Hello")
}
```

**main.go**

```
package main

import "sample.com/learn/hello"

func main() {
    hello.SayHello()
}
```

**Output**

```
Hello
```

In this program, we first import the **hello** package from the **main** package as below. 

```
import "sample.com/learn/hello"
```

Then we call the **SayHello** function as below from the **main** package

```
hello.SayHello()
```

This works because **SayHello** function is uppercase. 

Change the function to lower case and run this program. It gives the below compilation error

```
cannot refer to unexported name hello.sayHello
```

Also check out our Golang comprehensive tutorial Series – [Golang Comprehensive Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*