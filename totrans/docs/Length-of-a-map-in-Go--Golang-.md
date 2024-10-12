<!--yml
category: 未分类
date: 2024-10-13 06:19:13
-->

# Length of a map in Go (Golang)

> 来源：[https://golangbyexample.com/length-map-golang/](https://golangbyexample.com/length-map-golang/)

The golang builtin function **len()** can be used to get the length of the map which is number of key-value pairs present in the map. Below is the format for using this function on the map.

```
len(mapName)
```

Let’s see a program

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    employeeSalary["Sam"] = 1200

    lenOfMap := len(employeeSalary)
    fmt.Println(lenOfMap)
}
```

**Output**

```
2
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [length](https://golangbyexample.com/tag/length/)*   [map](https://golangbyexample.com/tag/map/)