<!--yml
category: 未分类
date: 2024-10-13 06:28:37
-->

# Const struct in Go (Golang)

> 来源：[https://golangbyexample.com/const-struct-go/](https://golangbyexample.com/const-struct-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Go only supports four types of constant

*   Numeric ( int, int64, float, float64, complex128 etc)
*   String
*   Character or rune
*   Boolean

It doesn’t support const struct. So below program would raise a compilation error

# **Example**

```
package main
import "fmt"
type employee struct {
    name string
    age  int
}
func main() {
    const e = employee{
        name: "John",
        age:  21,
    }
    fmt.Println(e)
}
```

**Output**

```
const initializer employee literal is not a constant
```

However workaround is to have a function which could return a struct. In a way that fulfills the purpose of a constant struct as it would return the same struct everytime

```
package main
import "fmt"
type employee struct {
    name string
    age  int
}
func main() {
    e := baseEmployee()
    fmt.Println(e)
}
func baseEmployee() employee {
    return employee{
        name: "Unnamed",
        age:  0,
    }
}
```

**Output**

```
{Unnamed 0}
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*