<!--yml

category: 未分类

date: 2024-10-13 06:21:07

-->

# 在 Go（Golang）中创建/初始化/声明 map

> 来源：[`golangbyexample.com/create-map-golang/`](https://golangbyexample.com/create-map-golang/)

目录

+   概述

+   声明一个 Map

+   创建一个 Map

    +   [使用 map[<key_type>]<value_type>格式](#Using_the_map_format "Using the map[<key_type>]<value_type>格式")

    +   使用 Make

# **概述**

map 是 Golang 内置的数据类型，类似于哈希表，它将键映射到值。

下面是 map 的格式：

```go
map[key_type]value_type
```

**key_type**和**value_type**可以是不同类型或相同类型。在下面的例子中，键类型是**string**，值类型是**int**

```go
map[string]int
```

# **声明一个 Map**

map 也可以使用 var 关键字声明，但它会创建一个 nil map，因为 map 的默认零值是 nil。向该 map 添加任何键值对将导致恐慌。让我们看看这个例子

```go
package main

func main() {
    var employeeSalary map[string]int
    employeeSalary["Tom"] = 2000
}
```

**输出**

```go
panic: assignment to entry in nil map
```

上面的程序因 map 为 nil 而导致恐慌。

使用**var**关键字声明 map 的一个用例是当需要将一个已存在的 map 赋值给它或当我们想要赋值函数的结果时。

# **创建一个 Map**

创建 map 的两种方式

+   使用 map[<key_type>]<value_type>{}格式也称为 map 字面量

+   使用 make

让我们逐一看看上述每种方法。

## **使用 map[<key_type>]<value_type>格式**

创建 map 的最常见方式之一是使用 map 字面量：

```go
map[key_type]value_type{}
```

上述例子的键类型是字符串，值类型是整数

```go
employeeSalary := map[string]int{}
```

map 也可以创建并初始化一些键值

```go
employeeSalary := map[string]int{
"John": 1000
"Sam": 2000
}
```

也可以向 map 中添加键值对

```go
employeeSalary["Tom"] = 2000
```

让我们看看一个程序

```go
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := map[string]int{}
    fmt.Println(employeeSalary)

    //Intialize using map lieteral
    employeeSalary = map[string]int{
        "John": 1000,
        "Sam":  1200,
    }

    //Adding a key value
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)
}
```

**输出**

```go
map[]
map[John:1000 Sam:1200 Tom:2000]
```

在上面的程序中，我们创建了一个初始化为某些值的 map 字面量。然后我们在其中添加了另一个键值对。接着我们使用**fmt.Println**打印它，以格式 map[key:value key:value]打印所有的键值对。

## **使用 Make**

这是创建 map 的另一种方式。内置函数**make**可用于创建 map。它返回一个初始化的 map。因此可以向其中添加键值对。

```go
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)
    //Adding a key value
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)
}
```

**输出**

```go
map[Tom:2000]
```

在上面的程序中，我们使用 make 函数创建了一个 map。然后我们在其中添加了一个键值对。接着我们使用**fmt.Println**打印它，打印出所有的键值对。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
