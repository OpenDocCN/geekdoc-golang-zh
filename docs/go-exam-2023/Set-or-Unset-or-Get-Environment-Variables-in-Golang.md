<!--yml

类别：未分类

日期：2024-10-13 06:09:29

-->

# 在Golang中设置、取消设置或获取环境变量

> 来源：[https://golangbyexample.com/set-unset-get-env-variable-golang/](https://golangbyexample.com/set-unset-get-env-variable-golang/)

目录

**   [概述](#Overview "概述")

    +   [设置环境值](#Setting_an_env_value "设置环境值")

    +   [获取环境值](#Get_an_env_value "获取环境值")

    +   [取消设置环境值](#Unset_an_env_value "取消设置环境值")

    +   [查找环境变量](#Lookup_for_an_env_variable "查找环境变量")

+   [代码：](#Code "代码：")*  *# **概述**

**os**包提供了一些与操作Go中的环境变量相关的实用方法。

## 设置环境值

```go
func Setenv(key, value string) error
```

## 获取环境值

如果环境变量的**Getenv()**函数返回的值为空，表示该环境变量不存在。要区分未设置和为空的环境变量，请使用LookupEnv()函数，它会返回额外的布尔值，指示环境变量是否存在。

```go
func Getenv(key string) string
```

## 取消设置环境值

```go
func Unsetenv(key string) error 
```

## 查找环境变量

从下面的签名可以明显看出，LookupEnv返回额外的布尔值。

+   如果**key**代表的环境变量存在，则返回其值，返回的布尔值为真。

+   如果key代表的环境变量不存在，则返回的值为空，返回的布尔值为假。

```go
func LookupEnv(key string) (string, bool)
```

# **代码：**

```go
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

**输出：**

```go
b
a env variable present: false
```*
