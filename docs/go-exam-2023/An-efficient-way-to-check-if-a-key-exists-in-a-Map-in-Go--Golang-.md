<!--yml

分类：未分类

日期：2024-10-13 06:19:19

-->

# 在 Go（Golang）中检查键是否存在于映射中的有效方法。

> 来源：[`golangbyexample.com/check-key-exists-map-golang/`](https://golangbyexample.com/check-key-exists-map-golang/)

以下是检查键是否存在于映射中的格式。

```go
val, ok := mapName[key]
```

有两种情况。

+   如果键存在，**val** 变量将是映射中键的值，而 **ok** 变量将为 true。

+   如果键不存在，**val** 变量将是值类型的默认零值，而 **ok** 变量将为 false。

让我们来看一个例子。

```go
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

```go
Key exists case
Val: 2000, ok: true
Key doesn't exists case
Val: 0, ok: false
```

在上面的程序中，当键存在时，**val** 变量被设置为实际值，这里是 2000，而 **ok** 变量为 true。当键不存在时，**val** 变量被设置为 0，这是 int 的默认零值，而 **ok** 变量为 false。这个 **ok** 变量是检查键是否存在于映射中的最佳方法。

如果我们只想检查键是否存在而不需要 val，那么可以用空标识符“_”替代 val。

```go
_, ok = employeeSalary["Sam"]
```

+   [存在](https://golangbyexample.com/tag/exists/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
