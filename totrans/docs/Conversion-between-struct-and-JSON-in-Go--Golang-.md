<!--yml
category: 未分类
date: 2024-10-13 06:33:31
-->

# Conversion between struct and JSON in Go (Golang)

> 来源：[https://golangbyexample.com/struct-json-golang/](https://golangbyexample.com/struct-json-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Struct to JSON](#Struct_to_JSON "Struct to JSON")
*   [JSON to struct](#JSON_to_struct "JSON to struct")*  *# **Overview**

**encoding/json** package provides utilities that can be used to convert to and from JSON. The same utility can be used to convert a golang struct to a JSON string and vice versa.  Two functions which are used are

*   Marshal – To convert a struct into JSON string

*   Unmarshal- To convert a JSON string back to struct

Before we see how to convert a struct to JSON and vice versa we need to know two things about golang struct:

*   Only exported fields of the struct are visible to the external library. Hence only exported fields of the struct will be present in the converted JSON string. Also to note that capitalized fields of a struct are exported in go.

*   Struct fields have a meta section that contains additional information about that field. These meta fields are used while converting a struct into JSON and vice-versa. Also, note that these struct meta fields is that they are optional. Let’s say we have the below struct

```
type employee struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}
```

Notice meta tags associated with each of the fields annotated with name as ‘json’. These meta fields are used while converting a struct into JSON and vice versa.

So the above struct when converted to JSON will be as below

```
{
  "name" : "John",
  "age"  : 21
}
```

Then **name **key of the above JSON string will map to the **Name** field of the **employee** struct and the **age** key in the JSON string will map to the **Age** field of the struct. Also, when converting the above JSON string  to a struct, the value in the **name** field of json will go to value in the **Name** field of the struct, and value in the **age** field of json will go to value in the **Age** field of struct .

As another example, let’s say we have below struct

```
type employee struct {
	Name string `json:"n"`
	Age  int    `json:"ag"`
}
```

Then after conversion to JSON,  **‘n’** key of the JSON will map to the **Name** field of the struct and **‘ag’ **key of the JSON will map to the **Age** field of the struct. So it will produce below JSON

```
{
  "n" : "John",
  "age"  : 21
}
```

Also, when converting the above JSON string  to a struct, the value in the **‘n’** field of JSON string will go to value in the **Name** field of the struct, and value in the **‘a’** field of json will go to value in the **Age** field of struct 

If the struct doesn’t contain any meta tags then the resulting key name in the JSON will be the same as the struct field’s name and vice versa. For eg if we have below struct

```
type employee struct {
	Name string 
	Age  int   
}
```

Notice then none of the fields has the JSON meta tags. So after converting to JSON it will be like

```
{
  "Name" : "John",
  "Age"  : 21
}
```

# **Struct to JSON**

**json.Marshal** function can be used to convert a struct to **JSON**. Let’s see an example of conversion from a struct to JSON. To illustrate all points above we have created two structs

*   **employee1** struct – It has meta tags

*   **employee2** struct – It doesn’t have meta tags

Also, the salary field in both the struct is unexported

```
package main
import (
    "encoding/json"
    "fmt"
    "log"
)
type employee1 struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    salary int    `json:"s"`
}
type employee2 struct {
    Name   string
    Age    int
    salary int
}
func main() {
    e1 := employee1{
        Name:   "John",
        Age:    21,
        salary: 1000,
    }
    j, err := json.Marshal(e1)
    if err != nil {
        log.Fatalf("Error occured during marshaling. Error: %s", err.Error())
    }
    fmt.Printf("employee1 JSON: %s\n", string(j))
    e2 := employee2{
        Name:   "John",
        Age:    21,
        salary: 1000,
    }
    j, err = json.Marshal(e2)
    if err != nil {
        log.Fatalf("Error occured during marshaling. Error: %s", err.Error())
    }
    fmt.Printf("\nemployee2 JSON: %s\n", string(j))
}
```

**Output**

```
employee1 JSON: {"n":"John","a":21}

employee2 JSON: {"Name":"John","Age":21}
```

Notice that we are using **json.Marshal** function to convert from the struct to JSON.

For the **employee1** struct to JSON conversion, the output is

```
{"n":"John","a":21}
```

This is because

*   **salary** field is not present in the output because it is not exported ie the field is not capitalized

*   Because of json tags associated with the **employee1** struct, the **‘Name’** field of the **employee1** struct is mapped to **‘n’** field of the JSON, and ‘**Age’** field is mapped to ‘**a’** field of the JSON

For the **employee2** struct to JSON conversion, the output is

```
{"Name":"John","Age":21}
```

This is because

*   **salary** field is not present in the output because it is not exported ie the field is not capitalized

*   Since no json tags are associated with the **employee2** struct, the **‘Name’** field of the **employee1** struct is mapped to **‘Name’** field of the JSON, and ‘**Age’** field is mapped to **‘Age’** field of the JSON

# **JSON to struct**

**json.Unmarshal** function can be used to convert from JSON to a struct. Whatever rules we discussed above also apply to conversion from JSON to a struct.  Let’s see an example

```
package main

import (
	"encoding/json"
	"fmt"
	"log"
)

type employee1 struct {
	Name   string `json:"n"`
	Age    int    `json:"a"`
	salary int    `json:"s"`
}

type employee2 struct {
	Name   string
	Age    int
	salary int
}

func main() {
	e1Json := `{"n":"John","a":21}`

	var e1Converted employee1
	err := json.Unmarshal([]byte(e1Json), &e1Converted)
	if err != nil {
		log.Fatalf("Error occured during unmarshaling. Error: %s", err.Error())
	}
	fmt.Printf("employee1 Struct: %#v\n", e1Converted)

	e2Json := `{"Name":"John","Age":21}`
	var e2Converted employee2
	err = json.Unmarshal([]byte(e2Json), &e2Converted)
	if err != nil {
		log.Fatalf("Error occured during unmarshaling. Error: %s", err.Error())
	}
	fmt.Printf("\nemployee2 Struct: %#v\n", e2Converted)
}
```

**Output**

```
employee1 Struct: main.employee1{Name:"John", Age:21, salary:0}

employee2 Struct: main.employee2{Name:"John", Age:21, salary:0}
```

This example uses the json string that was the output from the first example. Here we are using the **json.Unmarsha**l function to convert from JSON string to structThe first thing to note is that we need to pass the address of the struct to the **json.Unmarshal** function as below

```
err = json.Unmarshal(j, &e1Converted)
```

The first argument is the JSON bytes and the second is the address of the struct.

Unmarshing of

```
{"n":"John","a":21}
```

into **employee1** struct outputs

```
main.employee1{Name:"John", Age:21, salary:0}
```

Unmarshling of

```
{"Name":"John","Age":21}
```

into **employee2** struct outputs

```
main.employee2{Name:"John", Age:21, salary:0}
```

Also if you try unmarshalling

```
{"n":"John","a":21}
```

```
 into employee2 struct then the output will be 
```

```
main.employee2{Name:"", Age:0, salary:0}
```

Nothing will get unmarshalled because there are no meta tags in the **employee2** struct and key names in the **employee2** struct and key names in JSON is different. Hence an empty **employee2** struct will be created with each of the fields in the struct initialized with the default zero value of its type.

What if the JSON string contains the **salary** field. Then the value for the **salary** field in the JSON string will not reflect in the **salary** field of the struct because the **salary** field is not exported in the struct. See this example for the same.

```
package main
import (
    "encoding/json"
    "fmt"
    "log"
)
type employee1 struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    salary int    `json:"s"`
}
func main() {
    e1Json := `{"n":"John","a":21,"salary":1000}`
    var e1Converted employee1
    err := json.Unmarshal([]byte(e1Json), &e1Converted)
    if err != nil {
        log.Fatalf("Error occured during unmarshaling. Error: %s", err.Error())
    }
    fmt.Printf("employee1 Struct: %#v\n", e1Converted)
}
```

**Output**

```
employee1 Struct: main.employee1{Name:"John", Age:21, salary:0}
```

Even though the **salary** field in the JSON string has a value of 1000 but after converting it to the struct the **salary** field in the struct is 0.*