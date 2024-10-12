<!--yml
category: 未分类
date: 2024-10-13 06:33:21
-->

# Convert a JSON to map in Go (Golang)

> 来源：[https://golangbyexample.com/json-to-map-golang/](https://golangbyexample.com/json-to-map-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Examples](#Examples "Examples")*  *# **Overview**

**encoding/json** package provides utilities that can be used to convert to and from JSON. The same utility can be used to convert a golang map to JSON string and vice versa.

# **Examples**

**json.Unmarshal** function can be used to convert a JSON to the map

There can be two cases

*   If you know the format of the JSON, then initialize the map in the same format.  For eg when we know that the value part in the JSON is of string type then we can initialize the map in the below format

```
map[string]string
```

*   If the format of the JSON is not known then the corresponding map needs to be initialized in the below format. The value part needs to be an empty interface

```
map[string]interface{}
```

Let’s see some examples. In the first example, we have below JSON string

```
{"1":"John"}
```

Assume that the format of the JSON is known. Below is the program

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

**Output**

```
map[1:John]
```

In the second example, we have below JSON string

```
{"1":{"Name":"John"}}
```

We are parsing this JSON string into the below map type

```
map[int]employee
```

where the **employee** is a struct

```
type employee struct {
    Name string
}
```

This is the code

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

**Output**

```
map[1:{John}]
```

Let’s also see an example of the case where the format of the JSON is not known.

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

**Output**

```
John
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*