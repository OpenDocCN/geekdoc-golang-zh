<!--yml
category: 未分类
date: 2024-10-13 06:33:14
-->

# Convert a map to JSON in Go (Golang)

> 来源：[https://golangbyexample.com/map-to-json-golang/](https://golangbyexample.com/map-to-json-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

**encoding/json** package provides utilities that can be used to convert to and from JSON. The same utility can be used to convert a golang map to JSON string and vice versa. A very important point to note though is that map allows integer values for keys while JSON doesn’t allow integer values for keys. JSON only allows string value for keys. So a map having an integer value for the key when converted to JSON will have a string value for the key.

# **Example**

Let’s see a program for conversion of the map to JSON

```
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

**Output**

```
{"1":"John"}
```

In the above code, we are using **json.Marshal** function to convert the map to JSON. The map has an integer value for the key.

```
a := make(map[int]string)
```

While after converting, the resultant JSON as a string value for the key

```
{"1":"John"}
```

Let’s see one more example where we convert a map to a **JSON** where we have a struct for the value in the map. Below is the code for that

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

**Output**

```
{"1":{"Name":"John"}}
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*