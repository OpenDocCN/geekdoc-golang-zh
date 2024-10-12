<!--yml
category: 未分类
date: 2024-10-13 06:40:52
-->

# JSON parse a file in Go (Golang)

> 来源：[https://golangbyexample.com/json-parse-file-golang/](https://golangbyexample.com/json-parse-file-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [JSON parse a file into a struct](#JSON_parse_a_file_into_a_struct "JSON parse a file into a struct")
*   [JSON parse a file into a map](#JSON_parse_a_file_into_a_map "JSON parse a file into a map")*  *# **Overview**

**encoding/json** package provides an **Unmarshal** method that can be used to convert the file bytes into struct or map in golang.

**json.Unmarshal** function can be used to convert from JSON to a struct or app. Below is the signature of the method

```
func Unmarshal(data []byte, v interface{}) error
```

Let’s see examples of

*   JSON parse a file into a struct

*   JSON parse a file into a map

# **JSON parse a file into a struct**

Create a file **employee.json** with below contents

```
{"Name":"John","Age":21}
```

Below is the code

```
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

**Output**

```
emp Struct: main.employee{Name:"John", Age:21}
```

In the above code, we created an employee struct.

```
type employee struct {
	Name string `json:"Name"`
	Age  int    `json:"Age"`
}
```

The employee struct has some json meta tags which are used for converting json to a struct. See this link for more details – [https://golangbyexample.com/struct-field-meta-or-tags/](https://golangbyexample.com/struct-field-meta-or-tags/)

This is how we unmarshalled the file bytes into the employee instance.

```
err = json.Unmarshal([]byte(file), &emp)
```

# **JSON parse a file into a map**

A very important point to note though is that map allows integer values for keys while JSON doesn’t allow integer values for keys. JSON only allows string values for keys. So a map having an integer value for the key when converted to JSON will have a string value for the key.

Let’s say we have below json

```
{"1":"John", "2": "Simon"}
```

Below is the program that will read from the above and then convert the above json to map

```
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

**Output**

```
map[1:John 2:Simon]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*