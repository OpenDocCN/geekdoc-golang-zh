<!--yml
category: 未分类
date: 2024-10-13 06:27:53
-->

# Global and Local Constant in Go (Golang)

> 来源：[https://golangbyexample.com/global-local-constant-golang/](https://golangbyexample.com/global-local-constant-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Like any other variable, a constant will be global within a package if it is declared at the top of a file outside the scope of any function. For example, in the below program **name** will be a global constant available throughout the main package in any function. Do note that the const **name** will not be available outside the main package. For it to be available outside the main package it has to start with a capital letter. See the below code. It also shows the example of a local constant within a package.

# **Example**

```
package main

import "fmt"

const name = "test"

func main() {
    const a = 8
    fmt.Println(a)
    testGlobal()
}

func testGlobal() {
    fmt.Println(name)
    //The below line will give compiler error as a is a local constant
    //fmt.Println(a)
}
```

**Output**

```
8
test
```

Let’s see an example where we try to access the global constant from a different package. Let’s say that

*   **main.go** is present at location **$GOPATH/src/github.com/learn**

*   **data.go** is present at location **$GOPATH/src/github.com/learn/data**

**main.go**

```
package main

import (
	"fmt"

	"github.com/learn/data"
)

func main() {
	val := data.PI
	fmt.Println(val)
}
```

**data.go**

```
package data

const PI = 3.142
```

Go to the **learn** directory and run command

```
go run main.go
```

**Output**

```
3.142
```

In the above program we are trying to access the constant PI which is defined in data.go from the main package. The program works and successfully prints the value as the const PI starts with a capital letter and hence it is visible outside its package.

If you change the constant name from **PI** to **pi** then above program will raise compilation error

```
cannot refer to unexported name data.pi
./main.go:11:7: undefined: data.pi
```

Let’s see a program for that

**main.go**

```
package main

import (
	"fmt"

	"github.com/learn/data"
)

func main() {
	val := data.pi
	fmt.Println(val)
}
```

**data.go**

```
package data

const pi = 3.142
```

Go to the **learn** directory and run command

```
go run main.go
```

**Output**

```
cannot refer to unexported name data.pi
./main.go:11:7: undefined: data.pi
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*