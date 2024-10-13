<!--yml

类别：未分类

日期：2024-10-13 06:20:15

-->

# 在 Go（Golang）中对非结构体类型的方法

> 来源：[https://golangbyexample.com/method-non-struct-type-golang/](https://golangbyexample.com/method-non-struct-type-golang/)

方法也可以在非结构体自定义类型上定义。非结构体自定义类型可以通过类型定义创建。以下是创建新自定义类型的格式

```
type {type_name} {built_in_type}
```

例如，我们可以定义一个名为**myFloat**的自定义类型，其类型为**float64**。

```
type myFloat float64
```

可以在命名的自定义类型上定义方法。请看下面的示例：

**代码**

```
package main

import (
    "fmt"
    "math"
)

type myFloat float64

func (m myFloat) ceil() float64 {
    return math.Ceil(float64(m))
}

func main() {
    num := myFloat(1.4)
    fmt.Println(num.ceil())
}
```

**输出**

```
2
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [method](https://golangbyexample.com/tag/method/)
