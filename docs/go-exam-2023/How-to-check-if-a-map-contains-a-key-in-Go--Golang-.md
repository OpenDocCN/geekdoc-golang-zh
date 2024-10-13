<!--yml

分类：未分类

日期：2024-10-13 06:39:35

-->

# 如何检查 Go (Golang) 中的映射是否包含某个键

> 来源：[https://golangbyexample.com/check-map-key-golang/](https://golangbyexample.com/check-map-key-golang/)

以下是检查映射中是否存在键的格式。

```
val, ok := mapName[key]
```

有两种情况

+   如果键存在，**val** 变量将是映射中该键的值，而 **ok** 变量将为真。

+   如果键不存在，**val** 变量将是值类型的默认零值，而 **ok** 变量将为假。

让我们来看一个例子

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    fmt.Println("Key exists case")
    val, ok := employeeSalary["Tom"]
    fmt.Printf("Val: %d, ok: %t\n", val, ok)
    fmt.Println("Key doesn't exists case")

    val, ok = employeeSalary["Sam"]
    fmt.Printf("Val: %d, ok: %t\n", val, ok)
}
```

**输出**

```
Key exists case
Val: 2000, ok: true
Key doesn't exists case
Val: 0, ok: false
```

在上面的程序中，当键存在时，**val** 变量被设置为实际值，这里是 2000，**ok** 变量为真。当键不存在时，**val** 变量被设置为 0，这也是 int 的默认零值，**ok** 变量为假。这个 **ok** 变量是检查键是否存在于映射中的最佳方式。

如果我们只想检查一个键是否存在，而不需要 val，则可以用空标识符“_”代替 val。

```
_, ok = employeeSalary["Sam"]
```

另外，请查看我们的 Golang 进阶教程系列 – [Golang 进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
