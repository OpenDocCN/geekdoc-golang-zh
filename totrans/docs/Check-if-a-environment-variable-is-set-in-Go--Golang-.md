<!--yml
category: 未分类
date: 2024-10-13 06:09:47
-->

# Check if a environment variable is set in Go (Golang)

> 来源：[https://golangbyexample.com/check-environment-variable-set-go/](https://golangbyexample.com/check-environment-variable-set-go/)

**os.LookupEnv** function can be used to check whether a particular environment variable is set or not. It returns a bool which is true if the given env variable set otherwise false. Let’s see a working code:

```
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    //Set env a to b
    err := os.Setenv("a", "x")
    if err != nil {
        log.Fatal(err)
    }

    val, present := os.LookupEnv("a")
    fmt.Printf("a env variable present: %t\n", present)
    fmt.Println(val)

    val, present = os.LookupEnv("b")
    fmt.Printf("b env variable present: %t\n", present)
}
```

**Output:**

```
a env variable present: true
x
b env variable present: false
```

*   [env](https://golangbyexample.com/tag/env/)