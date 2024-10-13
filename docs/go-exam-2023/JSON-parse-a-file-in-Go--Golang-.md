<!--yml

类别：未分类

日期：2024-10-13 06:40:52

-->

# 在Go中解析JSON文件（Golang）

> 来源：[https://golangbyexample.com/json-parse-file-golang/](https://golangbyexample.com/json-parse-file-golang/)

目录

**   [概述](#Overview "概述")

+   [将JSON文件解析为结构体](#JSON_parse_a_file_into_a_struct "将JSON文件解析为结构体")

+   [将JSON文件解析为映射](#JSON_parse_a_file_into_a_map "将JSON文件解析为映射")*  *# **概述**

**encoding/json**包提供了一个**Unmarshal**方法，可以用于将文件字节转换为结构体或映射。

**json.Unmarshal**函数可用于将JSON转换为结构体或应用程序。以下是该方法的签名：

```go
func Unmarshal(data []byte, v interface{}) error
```

让我们看看以下示例：

+   将JSON文件解析为结构体

+   将JSON文件解析为映射

# **将JSON文件解析为结构体**

创建一个名为**employee.json**的文件，内容如下：

```go
{"Name":"John","Age":21}
```

以下是代码

```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"log"
)

type employee struct {
	Name string `json:"Name"`
	Age  int    `json:"Age"`
}

func main() {

	var emp employee
	file, err := ioutil.ReadFile("employee.json")
	if err != nil {
		log.Fatalf("Some error occured while reading file. Error: %s", err)
	}
	err = json.Unmarshal([]byte(file), &emp)
	if err != nil {
		log.Fatalf("Error occured during unmarshaling. Error: %s", err.Error())
	}
	fmt.Printf("emp Struct: %#v\n", emp)
}
```

**输出**

```go
emp Struct: main.employee{Name:"John", Age:21}
```

在上述代码中，我们创建了一个员工结构体。

```go
type employee struct {
	Name string `json:"Name"`
	Age  int    `json:"Age"`
}
```

员工结构体包含一些用于将JSON转换为结构体的元数据标签。有关更多详细信息，请查看此链接 – [https://golangbyexample.com/struct-field-meta-or-tags/](https://golangbyexample.com/struct-field-meta-or-tags/)

这就是我们如何将文件字节解析到员工实例中的。

```go
err = json.Unmarshal([]byte(file), &emp)
```

# **将JSON文件解析为映射**

需要注意的一点是，映射允许整数作为键，而JSON不允许整数作为键。JSON仅允许字符串作为键。因此，具有整数值作为键的映射在转换为JSON时，键将变为字符串值。

假设我们有以下JSON

```go
{"1":"John", "2": "Simon"}
```

以下是一个程序，将从上述内容读取并将上述JSON转换为映射：

```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"log"
)

func main() {
	a := make(map[int]string)

	file, err := ioutil.ReadFile("temp.json")
	if err != nil {
		log.Fatalf("Error occured during unmarshaling. Error: %s", err.Error())
	}
	json.Unmarshal([]byte(file), &a)

	fmt.Println(a)
}
```

**输出**

```go
map[1:John 2:Simon]
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
