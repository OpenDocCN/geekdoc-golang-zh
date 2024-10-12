<!--yml
category: 未分类
date: 2024-10-13 06:21:07
-->

# Create/Initialize/Declare map in Go (Golang)

> 来源：[https://golangbyexample.com/create-map-golang/](https://golangbyexample.com/create-map-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Declare a Map](#Declare_a_Map "Declare a Map")
*   [Creating a Map](#Creating_a_Map "Creating a Map")
    *   [Using the map[<key_type>]<value_type> format](#Using_the_map_format "Using the map[<key_type>]<value_type> format")
    *   [Using Make](#Using_Make "Using Make")*  *# **Overview**

Maps are golang builtin datatype similar to the hash table which maps a key to a value.

Below is the format for a map:

```
map[key_type]value_type
```

Both **key_type** and **value_type** can be of different type or same type. For below example the key type is **string** and value type is **int**

```
map[string]int
```

# **Declare a Map**

A map can also be declared with var keyword, but it creates a nil map as default zero value of map is nil. Adding any key value pair to that map will cause a panic. Let’s see an example for this

```
package main

func main() {
    var employeeSalary map[string]int
    employeeSalary["Tom"] = 2000
}
```

**Output**

```
panic: assignment to entry in nil map
```

Above program run into panic as the map is nil.

One use case of having a map declared with **var** keyword is when an already existing map needs to be assigned to it or when we want to assign the result of a function.

# **Creating a Map**

Two ways of creating a map

*   Using the map[<key_type>]<value_type>{} format also called map literal

*   Using make

Let’s look at each of the above methods one by one.

## **Using the map[<key_type>]<value_type> format**

One of the most common way of creating a map is using the map literal:

```
map[key_type]value_type{}
```

An example of above where key type is string and value type is integer

```
employeeSalary := map[string]int{}
```

A map can also be created with some key values initialized

```
employeeSalary := map[string]int{
"John": 1000
"Sam": 2000
}
```

A key-value pair can also be added to the map

```
employeeSalary["Tom"] = 2000
```

Let’s see a program

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := map[string]int{}
    fmt.Println(employeeSalary)

    //Intialize using map lieteral
    employeeSalary = map[string]int{
        "John": 1000,
        "Sam":  1200,
    }

    //Adding a key value
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)
}
```

**Output**

```
map[]
map[John:1000 Sam:1200 Tom:2000]
```

In above program we created a map literal intialized with some values. Then we added another key value pair in it. Then we printed it using **fmt.Println** which prints all the key value pairs in format map[key:value key:value]

## **Using Make**

This is another way of creating the map. The builtin function **make** can be used to create a map. It returns an initialized map. Hence key-value pairs can be added to it.

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)
    //Adding a key value
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)
}
```

**Output**

```
map[Tom:2000]
```

In above program we created a map using make function. Then we added a key value pair in it. Then we printed it using **fmt.Println** which prints all the key value pairs.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*