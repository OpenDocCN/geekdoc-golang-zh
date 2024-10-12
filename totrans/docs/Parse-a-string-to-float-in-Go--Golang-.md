<!--yml
category: 未分类
date: 2024-10-13 06:09:55
-->

# Parse a string to float in Go (Golang)

> 来源：[https://golangbyexample.com/parse-string-representation-float-go/](https://golangbyexample.com/parse-string-representation-float-go/)

**strconv.ParseFloat()** function can be used to parse a string representation of a float

[https://golang.org/pkg/strconv/#ParseFloat](https://golang.org/pkg/strconv/#ParseFloat)

Below is the signature of the function

```
func ParseFloat(s string, bitSize int) (float64, error) 
```

Some points worth noting

*   The first argument is the string representation of a float

*   The second argument is the bitSize which specifies the precision. It is 32 for float32 and 64 for float64

*   The return value will always be float64 but will be convertible to float32 without any change in its value.

Let’s see a working code

```
package main

import (
    "fmt"
    "strconv"
)

func main() {
    e1 := "1.3434"
    if s, err := strconv.ParseFloat(e1, 32); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }
    if s, err := strconv.ParseFloat(e1, 64); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }
}
```

**Output**

```
float64, 1.343400001525879
float64, 1.3434
```