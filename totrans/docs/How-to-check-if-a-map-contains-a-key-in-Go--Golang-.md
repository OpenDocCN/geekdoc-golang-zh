<!--yml
category: 未分类
date: 2024-10-13 06:39:35
-->

# How to check if a map contains a key in Go (Golang)

> 来源：[https://golangbyexample.com/check-map-key-golang/](https://golangbyexample.com/check-map-key-golang/)

Below is the format to check if a key exists in the map

```
val, ok := mapName[key]
```

There are two cases

*   If the key exists **val **variable be the value of the key in the map and **ok **variable will be true
*   If the key doesn’t exist **val** variable will be default zero value of value type and **ok **variable will be false

Let’s see an example

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    fmt.Println("Key exists case")
    val, ok := employeeSalary["Tom"]
    fmt.Printf("Val: %d, ok: %t\n", val, ok)
    fmt.Println("Key doesn't exists case")

    val, ok = employeeSalary["Sam"]
    fmt.Printf("Val: %d, ok: %t\n", val, ok)
}
```

**Output**

```
Key exists case
Val: 2000, ok: true
Key doesn't exists case
Val: 0, ok: false
```

In the above program when a key exists then **val** variable is set to the actual value which is 2000 here and ok variable is true. When the key doesn’t exist the val variable is set to 0 which is the default zero value of int and **ok** variable is false. This **ok** variable is the best way to check if the key exists in a map or not

In case we only want to check if a key is present and val is not needed, then blank identifier i.e “_” can be used in place of val.

```
_, ok = employeeSalary["Sam"]
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)