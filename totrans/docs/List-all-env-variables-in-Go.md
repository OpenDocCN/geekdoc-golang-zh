<!--yml
category: 未分类
date: 2024-10-13 06:09:33
-->

# List all env variables in Go

> 来源：[https://golangbyexample.com/list-all-env-variables-go/](https://golangbyexample.com/list-all-env-variables-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**os** package provides an **Environ()** function to get the list all environment variables.

*   Get all the env variables. It returns an array of string.

```
func Environ() []string 
```

There is also a way to clear all the env variables. **Clearenv()** function can be used to achieve the same.

```
func Clearenv()
```

# **Code:**

```
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    //Set env a to b
    err := os.Setenv("a", "b")
    if err != nil {
        log.Fatal(err)
    }

    err = os.Setenv("c", "d")
    if err != nil {
        log.Fatal(err)
    }

    //Get all env variables
    fmt.Println(os.Environ())

    //Clear all env variables
    os.Clearenv()

    //Again get all env variable
    fmt.Println(os.Environ())
}
```

**Output:**

```
List of all env varialbes on your system including a and b
```*