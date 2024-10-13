<!--yml

类别: 未分类

日期: 2024-10-13 06:33:14

-->

# 在Go中将映射转换为JSON

> 来源：[https://golangbyexample.com/map-to-json-golang/](https://golangbyexample.com/map-to-json-golang/)

目录

**   [概述](#Overview "概述")

+   [示例](#Example "示例")*  *# **概述**

**encoding/json**包提供了可以用于转换为JSON和从JSON转换的工具。相同的工具可以用来将Golang映射转换为JSON字符串，反之亦然。需要注意的一个重要点是，映射允许整数作为键，而JSON不允许整数作为键。JSON只允许字符串作为键。因此，具有整数值作为键的映射在转换为JSON时，键将是字符串值。

# **示例**

让我们看看将映射转换为JSON的程序

```go
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	a := make(map[int]string)

	a[1] = "John"

	j, err := json.Marshal(a)
	if err != nil {
		fmt.Printf("Error: %s", err.Error())
	} else {
		fmt.Println(string(j))
	}
}
```

**输出**

```go
{"1":"John"}
```

在上面的代码中，我们使用**json.Marshal**函数将映射转换为JSON。该映射的键是整数值。

```go
a := make(map[int]string)
```

转换后，结果JSON的键为字符串值

```go
{"1":"John"}
```

让我们再看一个示例，我们将一个映射转换为**JSON**，其中映射的值是一个结构体。以下是相应的代码

```go
package main
import (
    "encoding/json"
    "fmt"
)
type employee struct {
    Name string
}
func main() {
    a := make(map[string]employee)
    a["1"] = employee{Name: "John"}
    j, err := json.Marshal(a)
    if err != nil {
        fmt.Printf("Error: %s", err.Error())
    } else {
        fmt.Println(string(j))
    }
}
```

**输出**

```go
{"1":{"Name":"John"}}
```

+   [去](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*
