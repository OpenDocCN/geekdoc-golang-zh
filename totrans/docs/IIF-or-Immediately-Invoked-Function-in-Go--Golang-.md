<!--yml
category: 未分类
date: 2024-10-13 06:10:48
-->

# IIF or Immediately Invoked Function in Go (Golang)

> 来源：[https://golangbyexample.com/immediately-invoked-function-go/](https://golangbyexample.com/immediately-invoked-function-go/)

Table of Contents

 **   [Overview:](#Overview "Overview:")
*   [Use Case](#Use_Case "Use Case")
*   [Code:](#Code "Code:")*  *# **Overview:**

**IIF or** Immediately Invoked Function are those function which can be defined and executed at the same time. A function can be invoked immediately by appending a **()** after the end brace of the function.

# **Use Case**

One of the uses of the IIF function is when you don’t want to expose the logic of the function either within or outside the package. For eg let’s say there is a function which is setting some value. You can encapsulate all the logic of setting in an IIF function. This function won’t be available for calling either outside or within the package.

# **Code:**

Let’s see the working code. The function which computes the square of 2 is immediately invoked by having () at the end and the returned value is assigned to the variable **squareOf2**.

```
package main

import "fmt"

func main() {
    squareOf2 := func() int {
        return 2 * 2
    }()
    fmt.Println(squareOf2)
}
```

**Output:**

```
2
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [iif](https://golangbyexample.com/tag/iif/)*