<!--yml

类别：未分类

日期：2024-10-13 06:40:32

-->

# 将查询参数字符串转换为 Go（Golang）中的查询参数哈希

> 来源：[https://golangbyexample.com/query-param-map-golang/](https://golangbyexample.com/query-param-map-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

假设我们有以下查询参数字符串

```go
a=b&x=y
```

我们希望输出的格式如下图所示

```go
map[a:b x:y]
```

# **程序**

下面是相应的程序

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	query_param_map := make(map[string]string)

	input := "a=b&x=y"

	input_split := strings.Split(input, "&")

	for _, v := range input_split {
		v_split := strings.Split(v, "=")
		query_param_map[v_split[0]] = v_split[1]

	}
	fmt.Println(query_param_map)

}
```

**输出**

```go
map[a:b x:y]
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
