<!--yml
category: 未分类
date: 2024-10-13 06:22:57
-->

# Zero Value of Interface in Go (Golang)

> 来源：[https://golangbyexample.com/zero-value-of-interface-go/](https://golangbyexample.com/zero-value-of-interface-go/)

Default or zero value of an interface is nil. Below program demonstrates that

```
package main

import "fmt"
type animal interface {
    breathe()
    walk()
}

func main() {
    var a animal
    fmt.Println(a)
}
```

**Output**

```
nil
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)