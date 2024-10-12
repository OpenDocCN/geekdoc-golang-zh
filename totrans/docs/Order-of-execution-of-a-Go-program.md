<!--yml
category: 未分类
date: 2024-10-13 06:30:21
-->

# Order of execution of a Go program

> 来源：[https://golangbyexample.com/order-execution-program-golang/](https://golangbyexample.com/order-execution-program-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Below the order of execution of a go program.

*   The program starts with the main package.

*   All imported packages in the source files of the main package is initialized. The same thing happens recursively for further imported packages.

*   Then global variables declaration in these packages is initialized. The initialization dependency kicks in for the initialization of these variables. [https://golang.org/ref/spec#Order_of_evaluation](https://golang.org/ref/spec#Order_of_evaluation)

*   After this, init() function is run in these packages

*   Global variables in the main package are initialized

*   init function in the main package is run if present

*   main function in main package is run.

Note here that package initialization is only done once even if it is imported several times.

For example, if main package imports package **a** and in turn package **a** imports package **b**, then below will be the order

*   Global variables in package b will be initialized. init function in source files of package b will be run

*   Global variables in the package **a** will be initialized. init function in source files of the package **a** will be run

*   Global variables in main package will be initialized. init function in source files of main package will be run

*   main function will start executing.

# **Example**

Let’s see a program for the same.

**go.mod**

```
module sample

go 1.14
```

**sample/b/b1.go**

```
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b1")
}

func TestB() error {
	return nil
}
```

**sample/b/b2.go**

```
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b2")
}
```

**sample/a/a1.go**

```
package a

import (
	"fmt"
	"sample/b"
)

func init() {
	fmt.Println("Init: a1")
}

func TestA() error {
	return b.TestB()
}
```

**sample/a/a2.go**

```
package a

import (
	"fmt"
)

func init() {
	fmt.Println("Init: a2")
}
```

**sample/main.go**

```
package main

import (
	"fmt"
	"sample/a"
)

func init() {
	fmt.Println("Init: main")
}
func main() {
	fmt.Println("Main Function Executing")
	a.TestA()
}
```

**Output**

```
Init: b1
Init: b2
Init: a1
Init: a1
Init: main
Main Function Executing
```

Notice in above example that init function in source files of package **b** are run first. Then init function in source files of package **a** is run and then init function in source file of main package is run. After that main function is run

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*