<!--yml

分类：未分类

日期：2024-10-13 06:06:41

-->

# Go（Golang）中所有类型的默认零值及示例

> 来源：[https://golangbyexample.com/go-default-zero-value-all-types/](https://golangbyexample.com/go-default-zero-value-all-types/)

**默认值表**

| **类型** | **默认值** |
| --- | --- |
| 整数 | 0 |
| 浮点数 | 0 |
| 复数 | 0 实部和 0 虚部 |
| 字节 | 0 |
| 字符 | 0 |
| 字符串 | “” |
| 布尔值 | false |
| 数组 | 每个数组值都为其默认值 |
| 结构体 | 每个字段都为其默认值 |
| 映射 | nil |
| 通道 | nil |
| 接口 | nil |
| 切片 | nil |
| 指针 | nil |
| 函数 | nil |

# **示例：**

让我们看看每个的示例。使用侧边菜单进行导航。

## **整数**

```
package main

import "fmt"

func main() {
    var a int
    fmt.Print("Default zero value of int: ")
    fmt.Println(a)

    var b uint
    fmt.Print("Default zero value of uint: ")
    fmt.Println(b)
}
```

**输出：**

```
Default zero value of int: 0
Default zero value of uint: 0
```

## **浮点数**

```
package main
import "fmt"
func main() {
    var a float32
    fmt.Print("Default zero value of float: ")
    fmt.Println(a)
}
```

**输出：**

```
Default zero value of float: 0
```

## **复数**

```
package main
import "fmt"
func main() {
    var a complex64
    fmt.Print("Default zero value of complex: ")
    fmt.Println(a)
}
```

**输出：**

```
Default zero value of complex: (0+0i)
```

## **字节**

```
package main
import "fmt"
func main() {
    var a byte
    fmt.Print("Default zero value of byte: ")
    fmt.Println(a)
}
```

**输出：**

```
Default zero value of byte: 0
```

## **字符**

```
package main
import "fmt"
func main() {
    var a rune
    fmt.Print("Default zero value of rune: ")
    fmt.Println(a)
}
```

**输出：**

```
Default zero value of rune: 0
```

## **字符串**

```
package main
import "fmt"
func main() {
    var a string
    fmt.Print("Default zero value of string: ")
    fmt.Println(a)
}
```

**输出：**

```
Default zero value of string:    //An emtpy string. Hence nothing is printed
```

## **布尔值**

```
package main
import "fmt"
func main() {
    var a bool
    fmt.Print("Default zero value of bool: ")
    fmt.Println(a)
}
```

**输出：**

```
Default zero value of bool: false
```

## **数组**

数组的默认值是其值的默认值。例如，在下面的代码中，有一个长度为2的bool类型数组。当我们打印时，输出是**[false false]**。

```
package main
import "fmt"
func main() {
    var a [2]bool
    fmt.Print("Default Zero Value of an array: ")
    fmt.Println(a)
}
```

**输出：**

```
Default Zero Value of a array: [false false]
```

## **结构体**

**结构体**的默认值是其字段的默认值。例如，在下面的代码中，有一个包含两个字段的结构体样本。其中一个是int类型，另一个是bool类型。我们创建这个结构体的一个实例，当我们打印它时，输出是**{0 false}**。

```
package main
import "fmt"
func main() {
    type sample struct {
        a int
        b bool
    }
    a := sample{}
    fmt.Print("Default Zero Value of an struct: ")
    fmt.Println(a)
}
```

**输出：**

```
Default Zero Value of a struct: {0 false} 
```

## **映射**

**映射**的默认值是**nil**。这就是为什么**fmt.Println(a==nil)**的输出为true。当映射传递给**fmt.Println**时，它尝试打印映射中的值。这就是输出为map[]的原因。

```
package main
import "fmt"
func main() {
    var a map[bool]bool
    fmt.Println(a == nil)
    fmt.Print("Printing map: ")
    fmt.Println(a)
}
```

**输出：**

```
true
Printing map: map[]
```

## **通道**

```
package main
import "fmt"
func main() {
    var a chan int
    fmt.Print("Default zero value of channel: ")
    fmt.Println(a)
}
```

**输出：**

通道的默认零值：<nil>

## **接口**

```
package main
import "fmt"
func main() {
    var a chan interface{}
    fmt.Print("Default zero value of interface: ")
    fmt.Println(a)
}
```

**输出：**

接口的默认零值：<nil>

## **切片**

切片的默认值是**nil**。这就是为什么**fmt.Println(a==nil)**的输出为true。当切片传递给**fmt.Println**时，它尝试打印切片中的值。这就是输出为[]的原因。

```
package main
import "fmt"
func main() {
    var a []int
    fmt.Println(a == nil)
    fmt.Print("Printing slice: ")
    fmt.Println(a)
}
```

**输出：**

```
true
Printing slice: []
```

## **函数**

```
package main
import "fmt"
func main() {
    var a func()
    fmt.Print("Default Zero Value of a func: ")
    fmt.Println(a)
}
```

**输出：**

函数的默认零值：<nil>

## **指针**

```
package main
import "fmt"
func main() {
    var a *int
    fmt.Print("Default Zero Value of a pointer: ")
    fmt.Println(a)
}
```

**输出：**

指针的默认零值：<nil>

+   [数据类型](https://golangbyexample.com/tag/datatype/)   [默认值](https://golangbyexample.com/tag/default/)   [示例](https://golangbyexample.com/tag/examples/)   [go](https://golangbyexample.com/tag/go/)   [golang](https://golangbyexample.com/tag/golang/)   [侧边目录](https://golangbyexample.com/tag/sidetoc/)   [类型](https://golangbyexample.com/tag/type/)   [零值](https://golangbyexample.com/tag/zero/)
