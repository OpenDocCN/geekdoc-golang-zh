<!--yml

类别：未分类

日期：2024-10-13 06:27:49

-->

# Go（Golang）中的内层/外层作用域常量

> 来源：[https://golangbyexample.com/constant-scope-inner-outer-go/](https://golangbyexample.com/constant-scope-inner-outer-go/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

在内层作用域中声明的常量，如果与外层作用域中声明的常量同名，将会遮蔽外层作用域中的常量。

# **示例**

```
package main
import "fmt"
const a = 123
func main() {
    const a = 456
    fmt.Println(a)
}
```

**输出**

```
456
```

上述程序的输出为456，因为内层声明的常量具有该值

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
