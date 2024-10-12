<!--yml
category: 未分类
date: 2024-10-13 06:21:22
-->

# Scope of a variable in Go (Golang)

> 来源：[https://golangbyexample.com/scope-of-variable-go/](https://golangbyexample.com/scope-of-variable-go/)

**Scope of a Variable (Local and Global Variable)**

A variable declaration can be done at the package level or a function level or a block level. The scope of a variable defines from where that variable can be accessed and also the lifetime of the variable. Golang variables can be divided into two categories based on the scope.

*   Local Variable

*   Global Variable

**Local Variable:**

*   Local variables are variables which are defined within a block or a function level

*   An example of a block is a for loop or a range loop etc.

*   These variables are only be accessed from within their block or function

*   These variables only live till the end of the block or a function in which they are declared. After that, they are Garbage Collected.

*   A local once declared cannot be redeclared within the same block or function.

See below example

*   Local or loop variable **i** is not available after the for loop

*   Similarly local variable **aaa** is not available outside the function in which it is declared.

Hence below program will raise a compiler error

```
undefined: i
undefined: aaa.  #this occurs in the testLocal() function
```

**Code:**

```
package main

import "fmt"

func main() {
    var aaa = "test"
    fmt.Println(aaa)
    for i := 0; i < 3; i++ {
        fmt.Println(i)
    }
    fmt.Println(i)
}

func testLocal() {
    fmt.Println(aaa)
}
```

**Global Variable**

*   A variable will be global within a package if it is declared at the top of a file outside the scope of any function or block.

*   If this variable name starts with a lowercase letter then it can be accessed from within the package which contains this variable definition.

*   If the variable name starts with an uppercase letter then it can be accessed from an outside different package other than which it is declared.

*   Global variable are available throughout the lifetime of a program

For example, in the below program variable aaa will be a global variable available throughout the main package. It will be available in any function inside the **main** package. Do note that the variable name will not be available outside **main** package as its name starts with a lowercase letter.

```
package main

import "fmt"

var aaa = "test"

func main() {
    testGlobal()
}

func testGlobal() {
    fmt.Println(aaa)
}
```

**Output:**

```
test
```

Important Points

*   A variable declared within an inner scope having the same name as variable declared in the outer scope will shadow the variable in the outer scope.

```
package main

import "fmt"

var a = 123

func main() {
    var a = 456
    fmt.Println(a)
}
```

**Output:**

```
456
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [scope](https://golangbyexample.com/tag/scope/)*   [variable](https://golangbyexample.com/tag/variable/)