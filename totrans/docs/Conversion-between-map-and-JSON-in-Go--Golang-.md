<!--yml
category: 未分类
date: 2024-10-13 06:33:08
-->

# Conversion between map and JSON in Go (Golang)

> 来源：[https://golangbyexample.com/map-json-golang/](https://golangbyexample.com/map-json-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Map to JSON](#Map_to_JSON "Map to JSON")
*   [JSON to map](#JSON_to_map "JSON to map")*  *# **Overview**

**encoding/json** package provides utilities that can be used to convert to and from JSON. The same utility can be used to convert a golang map to JSON string and vice versa. A very important point to note though is that map allows integer values for keys while JSON doesn’t allow integer values for keys. JSON only allows string value for keys. So a map having an integer value for the key when converted to JSON will have a string value for the key.

# **Map to JSON**

Let’s see a program for conversion of map to JSON

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

In the above code, we are using **json.Marshal** function to convert the map to JSON. The map has an integer value for the key

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

# **JSON to map**

Let’s try to convert from JSON to map. We will convert the resultant JSON from the above example back to the map.

**json.Unmarshal** function can be used to convert a JSON back to the map

**First case:**

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

	var b map[int]string
	json.Unmarshal(j, &b)

	fmt.Println(b)
}
```

**Output**

```
{"1":"John"}
map[1:John]
```

**Second Case**

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
    var b map[int]employee
    json.Unmarshal(j, &b)
    fmt.Println(b)
}
```

**Output**

```
{"1":{"Name":"John"}}
map[1:{John}]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*