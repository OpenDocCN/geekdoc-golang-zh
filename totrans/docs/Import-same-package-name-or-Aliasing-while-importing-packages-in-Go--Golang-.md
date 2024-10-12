<!--yml
category: 未分类
date: 2024-10-13 06:30:49
-->

# Import same package name or Aliasing while importing packages in Go (Golang)

> 来源：[https://golangbyexample.com/import-same-package-name-golang/](https://golangbyexample.com/import-same-package-name-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Aliasing in importing packages means give a different name to the imported package. Syntax for it is

```
import <new_name> <directory_path></directory_path></new_name>
```

Above statement means that whatever package is present at directory <directory_path> import that package with name <new_name>. Aliasing is useful for giving

*   Giving a more relevant name to the imported package in the current context.

*   When two different import path contains the same package name then import one of them as a different name to prevent conflict.

# **Example**

Create a module with import path as [sample.com](http://sample.com)/learn

```
go mod init sample.com/learn
```

In below example we created two directory math and math2.

*   math directory contains file math.go with package declaration as

```
package math
```

*   math2 directory contains file math2.go with package declaration as

Notice that package name (i.e **math**) is same in folder **math** as well in folder **math2\.** So both folder **math2** and **math** contains the same package which is **math.** Since both directory have the same package name, aliasing is the only way to use both the package in the same file. This is how we alias and use both the package in main.go

```
import (
    "sample.com/learn/math"
    math2 "sample.com/learn/math2"
)
```

We alias **math** package present **“sample.com/learn/math2”** to **math2\.** If we haven’t done that then GO will raise a compilation issue as it cannot import package with same name from two different folders. This one of the advantage of using alias.

Let’s see full working code

**go.mod**

```
module sameple.com/learn

go 1.14
```

**learn/math2/math2.go**

```
package math
func Subtract(a, b int) int {
    return a - b
}
```

**learn/math/math.go**

```
package math
func Add(a, b int) int {
    return a + b
}
```

**learn/main.go**

```
package main
import (
    "fmt"
    "sample.com/learn/math"
    math2 "sample.com/learn/math2"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math2.Subtract(2, 1))
}
```

Let’s run this program

```
go install
learn $ learn
3
1
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*