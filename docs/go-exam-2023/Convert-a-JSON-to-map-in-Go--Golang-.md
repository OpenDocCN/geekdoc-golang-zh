<!--yml

类别：未分类

日期：2024-10-13 06:33:21

-->

# 在 Go (Golang) 中将 JSON 转换为 map。

> 来源：[https://golangbyexample.com/json-to-map-golang/](https://golangbyexample.com/json-to-map-golang/)

目录

**   [概述](#Overview "Overview")**

+   [示例](#Examples "Examples")*  *# **概述**

**encoding/json** 包提供的工具可以用于在 JSON 之间进行转换。相同的工具可以用于将 Golang map 转换为 JSON 字符串，反之亦然。

# **示例**

**json.Unmarshal** 函数可以用于将 JSON 转换为 map。

可能有两种情况。

+   如果你知道 JSON 的格式，那么请按相同格式初始化 map。例如，当我们知道 JSON 中的值部分为字符串类型时，我们可以按以下格式初始化 map。

```
map[string]string
```

+   如果 JSON 的格式未知，则需要按照以下格式初始化相应的 map。值部分需要是一个空接口。

```
map[string]interface{}
```

让我们看看一些示例。在第一个示例中，我们有以下 JSON 字符串。

```
{"1":"John"}
```

假设 JSON 的格式是已知的。以下是程序。

```
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	j := `{"1":"John"}`
	var b map[string]string
	json.Unmarshal([]byte(j), &b)

	fmt.Println(b)
}
```

**输出**

```
map[1:John]
```

在第二个示例中，我们有以下 JSON 字符串。

```
{"1":{"Name":"John"}}
```

我们正在将这个 JSON 字符串解析成下面的 map 类型。

```
map[int]employee
```

其中 **employee** 是一个结构体。

```
type employee struct {
    Name string
}
```

这是代码。

```
package main

import (
	"encoding/json"
	"fmt"
)

type employee struct {
	Name string
}

func main() {
	j := `{"1":{"Name":"John"}}`
	var b map[int]employee
	json.Unmarshal([]byte(j), &b)

	fmt.Println(b)
}
```

**输出**

```
map[1:{John}]
```

让我们再看看一种情况，即 JSON 的格式未知的例子。

```
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	j := `{"1":"John"}`
	var b map[string]interface{}
	json.Unmarshal([]byte(j), &b)

	fmt.Println(b["1"])
}
```

**输出**

```
John
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
