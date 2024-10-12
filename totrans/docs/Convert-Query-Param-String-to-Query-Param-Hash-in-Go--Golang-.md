<!--yml
category: 未分类
date: 2024-10-13 06:40:32
-->

# Convert Query Param String to Query Param Hash in Go (Golang)

> 来源：[https://golangbyexample.com/query-param-map-golang/](https://golangbyexample.com/query-param-map-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Assume we have below query param string

```
a=b&x=y
```

We want the output as a map as below

```
map[a:b x:y]
```

# **Program**

Below is the program for the same

```
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

**Output**

```
map[a:b x:y]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*