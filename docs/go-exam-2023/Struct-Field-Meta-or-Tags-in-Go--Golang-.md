<!--yml

category: 未分类

date: 2024-10-13 06:20:10

-->

# Go (Golang)中的结构体字段元或标签

> 来源：[`golangbyexample.com/struct-field-meta-or-tags/`](https://golangbyexample.com/struct-field-meta-or-tags/)

Go 中的结构体也允许向其字段添加元数据。这些元字段可以用于编码解码成不同形式，对结构体字段进行某些形式的验证等。因此，基本上可以与结构体的字段一起存储任何元信息，并可以被任何包或库用于不同的目的。

下面是附加元数据的格式。元数据是一个字符串字面量，即用反引号括起来。

```go
type strutName struct{
   fieldName type `key:value key2:value2`
}
```

让我们看一个 JSON 元或标签的示例。**Marshal**和**MarshalIndent**函数来自**encoding/json**包，可以用于以 JSON 格式打印结构体。这些函数在编码解码时利用结构体字段的 JSON 元标签。打印时，这些标签将用作键名。

让我们给员工结构体添加 JSON 标签，如下所示。**MarshalIndent**函数将使用标签中指定的键名。

```go
type employee struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    Salary int    `json:"s"`
}
```

让我们看看完整的程序

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
)

type employee struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    Salary int    `json:"s"`
}

func main() {
    emp := employee{Name: "Sam", Age: 31, Salary: 2000}
    //Converting to jsonn
    empJSON, err := json.MarshalIndent(emp, '', '  ')
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Println(string(empJSON))
}
```

**输出：**

```go
{
  "n": "Sam",
  "a": 31,
  "s": 2000
}
```

输出中的键名与 JSON 元标签中指定的相同。


