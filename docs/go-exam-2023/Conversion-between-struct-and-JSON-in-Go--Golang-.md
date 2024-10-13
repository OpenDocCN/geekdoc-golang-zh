<!--yml

category: 未分类

date: 2024-10-13 06:33:31

-->

# Go（Golang）中结构体与 JSON 之间的转换。

> 来源：[https://golangbyexample.com/struct-json-golang/](https://golangbyexample.com/struct-json-golang/)

目录

**   [概述](#Overview "Overview")

+   [结构体转 JSON](#Struct_to_JSON "Struct to JSON")

+   [JSON 转结构体](#JSON_to_struct "JSON to struct")*  *# **概述**

**encoding/json** 包提供了可以用于 JSON 转换的工具。相同的工具可以用于将 Go 结构体转换为 JSON 字符串及其反向转换。使用的两个函数是。

+   Marshal - 将结构体转换为 JSON 字符串。

+   Unmarshal - 将 JSON 字符串转换回结构体。

在我们查看如何将结构体转换为 JSON 及其反向转换之前，我们需要了解 Go 结构体的两件事：

+   只有结构体的导出字段对外部库可见。因此，在转换后的 JSON 字符串中仅会存在结构体的导出字段。还要注意，在 Go 中，结构体的大写字段是导出的。

+   结构体字段具有一个元部分，其中包含有关该字段的附加信息。这些元字段在将结构体转换为 JSON 及其反向转换时使用。此外，请注意这些结构体元字段是可选的。假设我们有以下结构体。

```go
type employee struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}
```

注意与每个字段关联的元标签，这些字段的名称为 ‘json’。在将结构体转换为 JSON 及其反向转换时使用这些元字段。

因此，上述结构体转换为 JSON 后将如下所示。

```go
{
  "name" : "John",
  "age"  : 21
}
```

那么上述 JSON 字符串中的 **name** 键将映射到 **employee** 结构体的 **Name** 字段，而 JSON 字符串中的 **age** 键将映射到结构体的 **Age** 字段。此外，在将上述 JSON 字符串转换为结构体时，JSON 中 **name** 字段的值将传递给结构体的 **Name** 字段，而 JSON 中 **age** 字段的值将传递给结构体的 **Age** 字段。

作为另一个示例，假设我们有以下结构体。

```go
type employee struct {
	Name string `json:"n"`
	Age  int    `json:"ag"`
}
```

然后在转换为 JSON 后，JSON 的 **‘n’** 键将映射到结构体的 **Name** 字段，而 JSON 的 **‘ag’** 键将映射到结构体的 **Age** 字段。因此，它将生成如下 JSON。

```go
{
  "n" : "John",
  "age"  : 21
}
```

此外，在将上述 JSON 字符串转换为结构体时，JSON 字符串中的 **‘n’** 字段的值将传递给结构体的 **Name** 字段，而 JSON 中 **‘a’** 字段的值将传递给结构体的 **Age** 字段。

如果结构体不包含任何元标签，则结果 JSON 中的键名将与结构体字段的名称相同，反之亦然。例如，如果我们有以下结构体。

```go
type employee struct {
	Name string 
	Age  int   
}
```

注意，字段没有 JSON 元数据标签。因此，转换为 JSON 后将如下所示。

```go
{
  "Name" : "John",
  "Age"  : 21
}
```

# **结构体转 JSON**

**json.Marshal** 函数可用于将结构体转换为 **JSON**。让我们来看一个从结构体转换为 JSON 的示例。为说明以上所有要点，我们创建了两个结构体。

+   **employee1** 结构体 - 它具有元标签。

+   **employee2**结构体 - 它没有元标签。

此外，两个结构体中的salary字段均为未导出。

```go
package main
import (
    "encoding/json"
    "fmt"
    "log"
)
type employee1 struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    salary int    `json:"s"`
}
type employee2 struct {
    Name   string
    Age    int
    salary int
}
func main() {
    e1 := employee1{
        Name:   "John",
        Age:    21,
        salary: 1000,
    }
    j, err := json.Marshal(e1)
    if err != nil {
        log.Fatalf("Error occured during marshaling. Error: %s", err.Error())
    }
    fmt.Printf("employee1 JSON: %s\n", string(j))
    e2 := employee2{
        Name:   "John",
        Age:    21,
        salary: 1000,
    }
    j, err = json.Marshal(e2)
    if err != nil {
        log.Fatalf("Error occured during marshaling. Error: %s", err.Error())
    }
    fmt.Printf("\nemployee2 JSON: %s\n", string(j))
}
```

**输出**

```go
employee1 JSON: {"n":"John","a":21}

employee2 JSON: {"Name":"John","Age":21}
```

请注意，我们使用**json.Marshal**函数将结构体转换为JSON。

对于**employee1**结构体到JSON的转换，输出为

```go
{"n":"John","a":21}
```

这是因为

+   **salary**字段在输出中不存在，因为它未导出，即该字段未大写。

+   由于与**employee1**结构体关联的JSON标签，**‘Name’**字段映射到JSON的**‘n’**字段，而**‘Age’**字段映射到JSON的**‘a’**字段。

对于**employee2**结构体到JSON的转换，输出为

```go
{"Name":"John","Age":21}
```

这是因为

+   **salary**字段在输出中不存在，因为它未导出，即该字段未大写。

+   由于**employee2**结构体没有关联任何JSON标签，**employee1**结构体的**‘Name’**字段映射到JSON的**‘Name’**字段，而**‘Age’**字段映射到JSON的**‘Age’**字段。

# **JSON到结构体**

**json.Unmarshal**函数可用于将JSON转换为结构体。我们之前讨论的规则同样适用于从JSON转换为结构体。让我们来看一个例子。

```go
package main

import (
	"encoding/json"
	"fmt"
	"log"
)

type employee1 struct {
	Name   string `json:"n"`
	Age    int    `json:"a"`
	salary int    `json:"s"`
}

type employee2 struct {
	Name   string
	Age    int
	salary int
}

func main() {
	e1Json := `{"n":"John","a":21}`

	var e1Converted employee1
	err := json.Unmarshal([]byte(e1Json), &e1Converted)
	if err != nil {
		log.Fatalf("Error occured during unmarshaling. Error: %s", err.Error())
	}
	fmt.Printf("employee1 Struct: %#v\n", e1Converted)

	e2Json := `{"Name":"John","Age":21}`
	var e2Converted employee2
	err = json.Unmarshal([]byte(e2Json), &e2Converted)
	if err != nil {
		log.Fatalf("Error occured during unmarshaling. Error: %s", err.Error())
	}
	fmt.Printf("\nemployee2 Struct: %#v\n", e2Converted)
}
```

**输出**

```go
employee1 Struct: main.employee1{Name:"John", Age:21, salary:0}

employee2 Struct: main.employee2{Name:"John", Age:21, salary:0}
```

此示例使用了第一个示例的输出JSON字符串。在这里，我们使用**json.Unmarshal**函数将JSON字符串转换为结构体。首先需要注意的是，我们需要将结构体的地址传递给**json.Unmarshal**函数，如下所示。

```go
err = json.Unmarshal(j, &e1Converted)
```

第一个参数是JSON字节，第二个是结构体的地址。

解组

```go
{"n":"John","a":21}
```

输出到**employee1**结构体。

```go
main.employee1{Name:"John", Age:21, salary:0}
```

解组

```go
{"Name":"John","Age":21}
```

输出到**employee2**结构体。

```go
main.employee2{Name:"John", Age:21, salary:0}
```

如果你尝试解组

```go
{"n":"John","a":21}
```

```go
 into employee2 struct then the output will be 
```

```go
main.employee2{Name:"", Age:0, salary:0}
```

由于**employee2**结构体中没有元标签，且**employee2**结构体中的键名与JSON中的键名不同，因此不会进行解组。因此将创建一个空的**employee2**结构体，结构体中的每个字段都将初始化为其类型的默认零值。

如果JSON字符串包含**salary**字段，那么JSON字符串中的**salary**字段的值将不会反映在结构体的**salary**字段中，因为**salary**字段在结构体中未导出。请参见此示例。

```go
package main
import (
    "encoding/json"
    "fmt"
    "log"
)
type employee1 struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    salary int    `json:"s"`
}
func main() {
    e1Json := `{"n":"John","a":21,"salary":1000}`
    var e1Converted employee1
    err := json.Unmarshal([]byte(e1Json), &e1Converted)
    if err != nil {
        log.Fatalf("Error occured during unmarshaling. Error: %s", err.Error())
    }
    fmt.Printf("employee1 Struct: %#v\n", e1Converted)
}
```

**输出**

```go
employee1 Struct: main.employee1{Name:"John", Age:21, salary:0}
```

尽管JSON字符串中的**salary**字段值为1000，但在转换为结构体后，结构体中的**salary**字段为0。
