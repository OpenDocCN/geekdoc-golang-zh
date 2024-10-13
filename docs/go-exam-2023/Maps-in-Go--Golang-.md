<!--yml

分类：未分类

日期：2024-10-13 06:19:06

-->

# Go 语言中的映射

> 来源：[https://golangbyexample.com/maps-in-golang/](https://golangbyexample.com/maps-in-golang/)

这是 Go 语言综合教程系列的第19章。请参阅此链接获取系列的其他章节 – [Go 语言综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [方法](https://golangbyexample.com/method-in-golang/)

**上一个教程** – [切片](https://golangbyexample.com/slice-in-golang/)

现在让我们看看当前的教程。以下是当前教程的目录。

目录

**   [概述](#Overview "概述")

+   [映射中允许的键类型](#Allowed_Key_types_in_a_Map "映射中允许的键类型")

+   [映射中允许的值类型](#Allowed_Value_types_in_a_Map "映射中允许的值类型")

+   [创建映射](#Creating_a_Map "创建映射")

    +   [使用 map[<key_type>]<value_type> 格式](#Using_the_map_format "使用 map[<key_type>]<value_type> 格式")

    +   [使用 Make](#Using_Make "使用 Make")

+   [映射操作](#Map_Operations "映射操作")

    +   [添加键值对](#Add_a_key_value_pair "添加键值对")

    +   [更新键值对](#Update_a_key-value_pair "更新键值对")

    +   [获取与键对应的值](#Get_the_value_corresponding_to_a_key "获取与键对应的值")

    +   [删除键值对](#Delete_a_key_value_pair "删除键值对")

    +   [检查键是否存在](#Check_if_a_key_exists "检查键是否存在")

    +   [映射上的函数](#Functions_on_Maps "映射上的函数")

+   [零值](#Zero_Value "零值")

+   [映射是引用数据类型](#Maps_are_referenced_data_types "映射是引用数据类型")

+   [遍历映射](#Iterate_over_a_map "遍历映射")

+   [映射不安全用于并发使用](#Maps_are_not_safe_for_concurrent_use "映射不安全用于并发使用")

+   [结论](#Conclusion "结论")*  *# **概述**

映射是 Go 语言内置的数据类型，类似于哈希表，用于将键映射到值。映射是一个无序集合，其中每个键是唯一的，而值可以对两个或多个不同的键相同。使用映射的优点是提供快速的检索、搜索、插入和删除操作。

映射是引用数据类型。当你将一个映射赋值给另一个映射时，它们都引用相同的基础映射。以下是映射的格式

```
map[key_type]value_type
```

**key_type** 和 **value_type** 可以是不同类型或相同类型。在以下示例中，键类型是 **string**，值类型是 **int**。

```
map[string]int
```

# **映射中允许的键类型**

映射键可以是任何可比较的类型。根据 Go 规范，一些可比较的类型是

+   布尔型

+   数值型

+   字符串,

+   指针

+   通道

+   接口类型

+   结构体 – 如果所有字段类型是可比较的

+   数组 – 如果数组元素值的类型是可比较的

根据 Go 规范，一些不可比较的类型不能作为映射中的键。

+   切片。

+   地图。

+   函数。

参考 – [https://golang.org/ref/spec#Comparison_operators](https://golang.org/ref/spec#Comparison_operators)

# **地图中的允许值类型**

值可以是地图中的任何类型。

# **创建一个地图**

+   使用map[<key_type>]<value_type>{}格式也称为地图字面量。

+   使用make。

让我们逐一看看上述每种方法。

## **使用map[<key_type>]<value_type>格式**

创建地图的最常见方法之一是使用地图字面量：

```
map[key_type]value_type{}
```

以上是一个示例，其中键类型为字符串，值类型为整数。

```
employeeSalary := map[string]int{}
```

地图也可以创建为一些已初始化的键值。

```
employeeSalary := map[string]int{
"John": 1000
"Sam": 2000
}
```

也可以向地图添加新的键值对。

```
employeeSalary["Tom"] = 2000
```

让我们看一个程序。

```
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

```
map[]
map[John:1000 Sam:1200 Tom:2000]
```

在上面的程序中，我们创建了一个初始化为某些值的地图字面量。然后我们在其中添加了另一个键值对。接着我们使用fmt.Println打印它，以**map[key:value key:value]**格式打印所有的键值对。

地图也可以使用var关键字声明，但它默认创建一个nil地图，因为地图的零值是nil。向该地图添加任何键值对将导致panic。让我们看一个例子。

```
package main

func main() {
    var employeeSalary map[string]int
    employeeSalary["Tom"] = 2000
}
```

**输出**

```
panic: assignment to entry in nil map
```

上述程序因地图为nil而引发panic。

使用**var**关键字声明地图的一个用例是，当需要将已有的地图赋值给它或当我们想要赋值函数的结果时。

## **使用Make**

这是另一种创建地图的方法。内置函数**make**可以用来创建地图。它返回一个已初始化的地图。因此，可以向其添加键值对。

```
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

```
map[Tom:2000]
```

在上面的程序中，我们使用make函数创建了一个地图。然后我们在其中添加了一个键值对。接着我们使用**fmt.Println**打印它，打印所有的键值对。

# **地图操作**

以下操作适用于地图。

+   添加一个键值对。

+   更新一个键。

+   获取与键对应的值。

+   删除一个键值对。

+   检查键是否存在。

## **添加一个键值对**

以下是向地图添加键值对的格式。

```
mapName[key] = value
```

让我们看一个例子。

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    fmt.Pr
```

**输出**

```
map[Tom:2000]
```

还要注意，向nil地图添加内容将导致panic。

## **更新一个键值对**

当尝试向已经存在的地图添加一个键时，新值将覆盖旧值。这类似于在地图中更新一个键。让我们看一个例子。

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    fmt.Println("Before update")
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)

    fmt.Println("After update")
    employeeSalary["Tom"] = 3000
    fmt.Println(employeeSalary)
}
```

**输出**

```
Before update
map[Tom:2000]
After update
map[Tom:3000]
```

在上面的程序中，写入相同的键**“Tom”**并赋新值**3000**时，将覆盖现有值**2000**。当我们再次打印地图时，打印的值为3000。

## **获取与键对应的值**

以下是检索与键对应的值的格式。

```
val := mapName[key]
```

让我们看一个程序。

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000

    //Retrieve a value 
    salary := employeeSalary["Tom"]
    fmt.Printf("Salary: %d", salary)
}
```

## **删除一个键值对**

以下是删除与键对应的值的格式。

```
delete(map_name, key)
```

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    fmt.Println("Adding key")
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)

    fmt.Println("\nDeleting key")
    delete(employeeSalary, "Tom")
    fmt.Println(employeeSalary)
}
```

**输出**

```
Adding key
map[Tom:2000]

Deleting key
map[]
```

在上面的程序中，我们删除了键，当我们再次打印地图时，该键不存在。

## **检查键是否存在**

以下是检查地图中是否存在某个键的格式。

```
val, ok := mapName[key]
```

有两种情况。

+   如果键存在，**val**变量将是地图中该键的值，**ok**变量将为true。

+   如果键不存在，**val**变量将是值类型的默认零值，**ok**变量为false

让我们看一个例子

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

在上述程序中，当键存在时，val变量被设置为实际值，这里是2000，ok变量为true。当**键**不存在时，**val**变量被设置为0，这是int类型的默认零值，**ok**变量为false。这个**ok**变量是检查键是否存在于映射中的最佳方式。

如果我们只想检查一个键是否存在，而不需要值，则可以使用空标识符，即“_”来代替值。

```
_, ok = employeeSalary["Sam"]
```

## **映射上的函数**

下面是可以在映射上使用的内置函数

+   len()函数

**len()函数**

**len()**函数可以用于获取映射的长度，即映射中存在的键值对的数量。下面是使用此函数的格式。

```
len(mapName)
```

让我们看一个程序

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    employeeSalary["Sam"] = 1200

    lenOfMap := len(employeeSalary)
    fmt.Println(lenOfMap)
}
```

**输出**

```
2
```

# **零值**

映射的零值是nil。当我们使用**var**关键字声明一个映射时，这一点也得到了证明。请参见下面的程序。

```
package main

import "fmt"

func main() {
    var employeeSalary map[string]int
    if employeeSalary == nil {
        fmt.Println("employeeSalary map is nil")
    }
}
```

**输出**

```
employeeSalary map is nil
```

# **映射是引用数据类型**

映射是引用数据类型。因此，将一个映射分配给一个新变量时，这两个变量都引用同一个映射。一个映射的任何更改都会反映在另一个映射中，反之亦然。

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    employeeSalary["Sam"] = 1200

    eS := employeeSalary

    //Change employeeSalary
    employeeSalary["John"] = 3000
    fmt.Println("Changing employeeSalary Map")
    fmt.Printf("employeeSalary: %v\n", employeeSalary)
    fmt.Printf("eS: %v\n", eS)

    //Change eS
    employeeSalary["John"] = 4000
    fmt.Println("\nChanging eS Map")
    fmt.Printf("employeeSalary: %v\n", employeeSalary)
    fmt.Printf("eS: %v\n", eS)
}
```

在**上述程序**中，eS是一个新映射变量，我们将现有的**employeeSalary**映射分配给它。

+   首先，我们在**employeeSalary**映射中添加一个新键。这个更改同时反映在**employeeSalary**和**eS**映射中

+   其次，我们更新了**eS**映射中的一个现有键。这个更改同样反映在**employeeSalary**和**eS**映射中。

这表明映射是引用数据类型

# **遍历映射**

range操作符可以用于在Go中遍历映射

首先让我们定义一个映射

```
sample := map[string]string{
        "a": "x",
        "b": "y",
}
```

+   **迭代所有键和值**

```
for k, v := range sample {
   fmt.Printf("key :%s value: %s\n", k, v)
}
```

**输出：**

```
key :a value: x
key :b value: y
```

+   **仅迭代键**

```
for k := range sample {
   fmt.Printf("key :%s\n", k)
}
```

**输出：**

```
key :a
key :b
```

+   **仅迭代值**

```
for _, v := range sample {
   fmt.Printf("value :%s\n", v)
}
```

**输出：**

```
value :x
value :y
```

+   **获取所有键的列表**

```
keys := getAllKeys(sample)
fmt.Println(keys)

func getAllKeys(sample map[string]string) []string {
    var keys []string
    for k := range sample {
        keys = append(keys, k)
    }
    return keys
}
```

**输出：**

```
[a b]
```

# **映射在并发使用时不安全**

golang映射在并发使用时是不安全的。

**有错误的代码：** 下面是一段有错误的代码。如果发生并发读取和写入映射，可能会导致崩溃。

```
package main

var (
   allData = make(map[string]string)
)

func get(key string) string {
    return allData[key]
}

func set(key string, value string) {
    allData[key] = value
}

func main() {
    go set("a", "Some Data 1")
    go set("b", "Some Data 2")
    go get("a")
    go get("b")
    go get("a")
}
```

**可能的输出：**

```
fatal error: concurrent map read and map write
```

**正确的代码：**

我们可以使用锁来允许并发访问映射

```
package main

import (
    "fmt"
    "sync"
)

var (
    allData = make(map[string]string)
    rwm     sync.RWMutex
)

func get(key string) string {
    rwm.RLock()
    defer rwm.RUnlock()
    return allData[key]

}

func set(key string, value string) {
    rwm.Lock()
    defer rwm.Unlock()
    allData[key] = value

}

func main() {
    set("a", "Some Data")
    result := get("a")
    fmt.Println(result)
}
```

**输出**

```
Some data
```

# **结论**

这就是关于golang中映射的全部内容。我们学习了如何创建映射、映射上的操作、映射中定义的一些函数，如Glen()，以及如何遍历映射，最后但同样重要的是，映射在并发使用时是不安全的。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一篇教程** – [方法](https://golangbyexample.com/method-in-golang/)

**上一篇教程** – [切片](https://golangbyexample.com/slice-in-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [maps](https://golangbyexample.com/tag/maps/)*
