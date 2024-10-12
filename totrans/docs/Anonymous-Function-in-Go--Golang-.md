<!--yml
category: 未分类
date: 2024-10-13 06:11:11
-->

# Anonymous Function in Go (Golang)

> 来源：[https://golangbyexample.com/go-anonymous-function/](https://golangbyexample.com/go-anonymous-function/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

As the name suggests anonymous functions are function which does not have any name.  In Golang function are first-class variables meaning that

*   They can be assigned to a variable
*   Passed around as function argument
*   Returned from a function

A function in Go is a first-class variable so it can be used as a value as well. When using a function as a value, it is not named and can be assigned to a variable. Such a function is called anonymous functions because the function is not named.

They are generally created for short term use or for limited functionality. See the below example.

In this example, a function is assigned to the variable **max**. The function assigned to **max** does not have any name. The only way to call this function is using the **max** variable and that is what we are doing in this program as well.

Also notice that we can pass arguments to the anonymous function as well as return values from them.

# **Code**

```
package main

import "fmt"

var max = func(a, b int) int {
    if a >= b {
        return a
    }
    return b
}

func main() {
    res := max(2, 3)
    fmt.Println(res)
}
```

**Output:**

```
3
```

The anonymous function can also be executed as IIF or Immediately Invoked Function. In such a case, you don’t need to assign it to any variable. See below example:

```
package main

import "fmt"

func main() {
    func() {
        fmt.Println("From anoymous function")
    }()
}
```

**Output:**

```
From anoymous function
```

*   [golang](https://golangbyexample.com/tag/golang/)*