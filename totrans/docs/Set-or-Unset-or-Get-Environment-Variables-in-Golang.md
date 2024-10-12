<!--yml
category: 未分类
date: 2024-10-13 06:09:29
-->

# Set or Unset or Get Environment Variables in Golang

> 来源：[https://golangbyexample.com/set-unset-get-env-variable-golang/](https://golangbyexample.com/set-unset-get-env-variable-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
    *   [Setting an env value](#Setting_an_env_value "Setting an env value")
    *   [Get an env value](#Get_an_env_value "Get an env value")
    *   [Unset an env value](#Unset_an_env_value "Unset an env value")
    *   [Lookup for an env variable](#Lookup_for_an_env_variable "Lookup for an env variable")
*   [Code:](#Code "Code:")*  *# **Overview**

**os** package provides a couple of utility methods related to playing around env variables in Go.

## Setting an env value

```
func Setenv(key, value string) error
```

## Get an env value

The value returned by the **Getenv()** function will be empty if that env variable is not present. To differentiate between an unset and empty env variable use LookupEnv() function which an additional boolean indicating if the env variable was present or not.

```
func Getenv(key string) string
```

## Unset an env value

```
func Unsetenv(key string) error 
```

## Lookup for an env variable

As evident from the below signature, LookupEnv returns additional boolean.

*   If the environment variable represented by the **key** is present, then its value is returned and the bool returned is true.
*   If the environment variable represented by the key is not present, then the value is empty and the bool returned is false.

```
func LookupEnv(key string) (string, bool)
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

    //Get env a
    val := os.Getenv("a")
    fmt.Println(val)

    //Unset a
    err = os.Unsetenv("a")
    if err != nil {
        log.Fatal(err)
    }

    val, present := os.LookupEnv("a")
    fmt.Printf("a env variable present: %t\n", present)
}
```

**Output:**

```
b
a env variable present: false
```*