<!--yml
category: 未分类
date: 2024-10-13 06:20:51
-->

# Update a key in the map in Go (Golang)

> 来源：[https://golangbyexample.com/update-key-map-golang/](https://golangbyexample.com/update-key-map-golang/)

When trying to add a key to the map which already exists, the new value will override the old value. This is analogous to updating a key in the map.

Let’s see an example

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    fmt.Println("Before update")
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)

    fmt.Println("After update")
    employeeSalary["Tom"] = 3000
    fmt.Println(employeeSalary)
}
```

**Output**

```
Before update
map[Tom:2000]
After update
map[Tom:3000]
```

In the above program after writing the same key **“Tom”** with a new value of **3000** it overwrites the existing value of **2000\.** When we print the map again the value printed is 3000

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)