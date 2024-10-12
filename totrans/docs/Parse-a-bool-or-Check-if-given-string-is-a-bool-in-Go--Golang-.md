<!--yml
category: 未分类
date: 2024-10-13 06:10:00
-->

# Parse a bool or Check if given string is a bool in Go (Golang)

> 来源：[https://golangbyexample.com/check-string-bool-golang/](https://golangbyexample.com/check-string-bool-golang/)

**strconv.ParseBool()** function can be used to parse a string representation of a bool.

[https://golang.org/pkg/strconv/#ParseBool](https://golang.org/pkg/strconv/#ParseBool)

Below is the signature of the function

```
func ParseBool(str string) (bool, error)
```

Let’s see a working code

```
package main

import (
    "fmt"
    "strconv"
)

func main() {
    input := "true"
    if val, err := strconv.ParseBool(input); err == nil {
        fmt.Printf("%T, %v\n", val, val)
    }

    input = "false"
    if val, err := strconv.ParseBool(input); err == nil {
        fmt.Printf("%T, %v\n", val, val)
    }

    input = "garbage"
    if val, err := strconv.ParseBool(input); err == nil {
        fmt.Printf("%T, %v\n", val, val)
    } else {
        fmt.Println("Given input is not a bool")
    }
}
```

**Output:**

```
bool, true
bool, false
Given input is not a bool
```

*   [boolean](https://golangbyexample.com/tag/boolean/)*   [go](https://golangbyexample.com/tag/go/)