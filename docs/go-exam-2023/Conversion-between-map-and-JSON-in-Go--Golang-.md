<!--yml

分类：未分类

日期：2024-10-13 06:33:08

-->

# 在 Go (Golang)中，映射与 JSON 之间的转换。

> 来源：[`golangbyexample.com/map-json-golang/`](https://golangbyexample.com/map-json-golang/)

目录

+   概述

+   映射到 JSON

+   JSON 到映射

# **概述**

**encoding/json**包提供了用于转换到 JSON 和从 JSON 转换的工具。可以使用相同的工具将 Golang 映射转换为 JSON 字符串，反之亦然。但需要注意的是，映射允许键的整数值，而 JSON 不允许键的整数值。JSON 仅允许键的字符串值。因此，当将具有整数键值的映射转换为 JSON 时，键将具有字符串值。

# **映射到 JSON**

让我们来看一个将映射转换为 JSON 的程序。

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

在上面的代码中，我们使用**json.Marshal**函数将映射转换为 JSON。该映射的键对应一个整数值。

```go
a := make(map[int]string)
```

在转换后，结果 JSON 作为键的字符串值。

```go
{"1":"John"}
```

让我们再看一个例子，在这个例子中，我们将映射转换为**JSON**，其中映射的值是一个结构体。以下是相关代码。

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

# **JSON 到映射**

让我们尝试将 JSON 转换为映射。我们将把上面示例中得到的 JSON 结果转换回映射。

**json.Unmarshal**函数可用于将 JSON 转换回映射。

**第一种情况：**

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

	var b map[int]string
	json.Unmarshal(j, &b)

	fmt.Println(b)
}
```

**输出**

```go
{"1":"John"}
map[1:John]
```

**第二种情况**

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
    var b map[int]employee
    json.Unmarshal(j, &b)
    fmt.Println(b)
}
```

**输出**

```go
{"1":{"Name":"John"}}
map[1:{John}]
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
