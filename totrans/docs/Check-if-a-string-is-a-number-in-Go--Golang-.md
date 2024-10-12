<!--yml
category: 未分类
date: 2024-10-13 06:09:51
-->

# Check if a string is a number in Go (Golang)

> 来源：[https://golangbyexample.com/check-if-string-is-number-golang/](https://golangbyexample.com/check-if-string-is-number-golang/)

**strconv.Atoi()** function can be used to check if the string is a number. If this function returns an error if the passed string is not a number. This function parses the number to base 10

[https://golang.org/pkg/strconv/#Atoi](https://golang.org/pkg/strconv/#Atoi)

Signature of the function is

```
func Atoi(s string) (int, error)
```

**Working Code:**

```
package main

import (
    "fmt"
    "strconv"
)
func main() {
    x := "1234"
    val, err := strconv.Atoi(x)
    if err != nil {
        fmt.Printf("Supplied value %s is not a number\n", x)
    } else {
        fmt.Printf("Supplied value %s is a number with value %d\n", x, val)
    }

    y := "123b"
    val, err = strconv.Atoi(y)
    if err != nil {
        fmt.Printf("Supplied value %s is not a number\n", y)
    } else {
        fmt.Printf("Supplied value %s is a number with value %d\n", y, val)
    }
}
```

**Output:**

```
Supplied value 1234 is a number with value 1234
Supplied value 123b is not a number
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)