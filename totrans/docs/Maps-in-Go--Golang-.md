<!--yml
category: 未分类
date: 2024-10-13 06:19:06
-->

# Maps in Go (Golang)

> 来源：[https://golangbyexample.com/maps-in-golang/](https://golangbyexample.com/maps-in-golang/)

This is the  chapter 19 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Method](https://golangbyexample.com/method-in-golang/)
**Previous Tutorial** – [Slice](https://golangbyexample.com/slice-in-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Allowed Key types in a Map](#Allowed_Key_types_in_a_Map "Allowed Key types in a Map")
*   [Allowed Value types in a Map](#Allowed_Value_types_in_a_Map "Allowed Value types in a Map")
*   [Creating a Map](#Creating_a_Map "Creating a Map")
    *   [Using the map[<key_type>]<value_type> format](#Using_the_map_format "Using the map[<key_type>]<value_type> format")
    *   [Using Make](#Using_Make "Using Make ")
*   [Map Operations](#Map_Operations "Map Operations")
    *   [Add a key value pair](#Add_a_key_value_pair "Add a key value pair")
    *   [Update a key-value pair](#Update_a_key-value_pair "Update a key-value pair")
    *   [Get the value corresponding to a key](#Get_the_value_corresponding_to_a_key "Get the value corresponding to a key")
    *   [Delete a key value pair](#Delete_a_key_value_pair "Delete a key value pair")
    *   [Check if a key exists](#Check_if_a_key_exists "Check if a key exists")
    *   [Functions on Maps](#Functions_on_Maps "Functions on Maps")
*   [Zero Value](#Zero_Value "Zero Value")
*   [Maps are referenced data types](#Maps_are_referenced_data_types "Maps are referenced data types")
*   [Iterate over a map](#Iterate_over_a_map "Iterate over a map")
*   [Maps are not safe for concurrent use](#Maps_are_not_safe_for_concurrent_use "Maps are not safe for concurrent use")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

Maps are golang builtin datatype similar to the hash table which maps a key to a value. Map is an unordered collection where each key is unique while values can be the same for two or more different keys. The advantages of using a map are that it provides fast retrieval, search, insert, and delete operations.

Maps are referenced data types. When you assign one map to another both refer to the same underlying map. Below is the format for a map

```
map[key_type]value_type
```

Both **key_type** and **value_type** can be of different type or same type. For below example the key type is **string** and value type is **int**.

```
map[string]int
```

# **Allowed Key types in a Map**

The map key can be any type that is comparable. Some of the comparable types as defined by go specification are

*   boolean
*   numeric
*   string,
*   pointer
*   channel
*   interface types
*   structs – if all it’s field type is comparable
*   array – if the type of value of array element is comparable

Some of the types which are not comparable as per go specification and which cannot be used as a key in a map are.

*   Slice
*   Map
*   Function

Reference – [https://golang.org/ref/spec#Comparison_operators](https://golang.org/ref/spec#Comparison_operators)

# **Allowed Value types in a Map**

Value can be of any type in a map.

# **Creating a Map**

*   Using the map[<key_type>]<value_type>{} format also called map literal

*   Using make

Let’s look at each of above method one by one.

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

A new key-value pair can also be added to the map

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

In the above program, we created a map literal intialized with some values. Then we added another key-value pair in it. Then we printed it using fmt.Println which prints all the key-value pairs in format **map[key:value key:value]**

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

## **Using Make**

Thisis another way of creating the map. The builtin function **make** can be used to create a map. It returns an initialized map. Hence key-value pairs can be added to it.

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

# **Map Operations**

The below operations are applicable for map

*   Add a key-value pair
*   Update a key
*   Get the value corresponding to a key
*   Delete a key-value pair
*   Check if a key exists

## **Add a key value pair**

Below is the format for adding a key value pair to a map

```
mapName[key] = value
```

Let’s see an example

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    fmt.Pr
```

**Output**

```
map[Tom:2000]
```

Also note that adding to a nil map will cause a panic.

## **Update a key-value pair**

When trying to add a key to the map which already exists, the new value will override the old value. This is analogous to updating a key in the map. Let’s see an example

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    fmt.Println("Before update")
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)

    fmt.Println("After update")
    employeeSalary["Tom"] = 3000
    fmt.Println(employeeSalary)
}
```

**Output**

```
Before update
map[Tom:2000]
After update
map[Tom:3000]
```

In the above program after writing the same key **“Tom”** with a new value of **3000** it overwrites the existing value of **2000\.** When we print the map again the value printed is 3000

## **Get the value corresponding to a key**

Below is the format for retrieving a value corresponding to a key

```
val := mapName[key]
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

    //Retrieve a value 
    salary := employeeSalary["Tom"]
    fmt.Printf("Salary: %d", salary)
}
```

## **Delete a key value pair**

Below is the format for delete a value corresponding to a key

```
delete(map_name, key)
```

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    fmt.Println("Adding key")
    employeeSalary["Tom"] = 2000
    fmt.Println(employeeSalary)

    fmt.Println("\nDeleting key")
    delete(employeeSalary, "Tom")
    fmt.Println(employeeSalary)
}
```

**Output**

```
Adding key
map[Tom:2000]

Deleting key
map[]
```

In above program we delete the key and when we print the map again, the key is not there.

## **Check if a key exists**

Below is the format to check if a key exist in the map

```
val, ok := mapName[key]
```

There are two cases

*   If the key exists **val** variable be the value of the key in the map and **ok** variable will be true

*   If the key doesn’t exist **val** variable will be default zero value of value type and **ok** variable will be false

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

In the above program when key exists then val variable is set to the actual value which is 2000 here and ok variable is true. When the **key** doesn’t exist, the **val** variable is set to 0 which is the default zero value of int and **ok** variable is false. This **ok** variable is the best way to check if the key exists in a map or not

In case we only want to check if a key is present and val is not needed, then blank identifier i.e “_” can be used in place of val.

```
_, ok = employeeSalary["Sam"]
```

## **Functions on Maps**

Below is the builtin function which can be used on a map

*   len() function

**len() function**

The **len()** function can be used to get the length of the map which is number of key value pair present in the map. Below is the format for using this function on map.

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

# **Zero Value**

zero value of a map is nil. This is also proved when we declare a map using the **var** keyword. See below program.

```
package main

import "fmt"

func main() {
    var employeeSalary map[string]int
    if employeeSalary == nil {
        fmt.Println("employeeSalary map is nil")
    }
}
```

**Output**

```
employeeSalary map is nil
```

# **Maps are referenced data types**

Map are reference data types. So on assigning one map to a new variable, then both variable refers to the same map. Any change in one of the map would reflect in other and vice versa.

```
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    employeeSalary["Sam"] = 1200

    eS := employeeSalary

    //Change employeeSalary
    employeeSalary["John"] = 3000
    fmt.Println("Changing employeeSalary Map")
    fmt.Printf("employeeSalary: %v\n", employeeSalary)
    fmt.Printf("eS: %v\n", eS)

    //Change eS
    employeeSalary["John"] = 4000
    fmt.Println("\nChanging eS Map")
    fmt.Printf("employeeSalary: %v\n", employeeSalary)
    fmt.Printf("eS: %v\n", eS)
}
```

In**the** above program, eS is a new map variable to which we assign the existing **employeeSalary** map.

*   First, we add a new key in **employeeSalary** map. The change reflects both in **employeeSalary** and **eS** map

*   Second, we updated an existing key in **eS** map. The change again reflects both in **employeeSalary** and **eS** map.

This shows that maps are referenced data type

# **Iterate over a map**

Range operator can be used to iterate over a map in Go

Let’s define a map first

```
sample := map[string]string{
        "a": "x",
        "b": "y",
}
```

*   **Iterating over all keys and values**

```
for k, v := range sample {
   fmt.Printf("key :%s value: %s\n", k, v)
}
```

**Output:**

```
key :a value: x
key :b value: y
```

*   **Iterating over only keys**

```
for k := range sample {
   fmt.Printf("key :%s\n", k)
}
```

**Output:**

```
key :a
key :b
```

*   **Iterating over only values**

```
for _, v := range sample {
   fmt.Printf("value :%s\n", v)
}
```

**Output:**

```
value :x
value :y
```

*   **Get list of all keys**

```
keys := getAllKeys(sample)
fmt.Println(keys)

func getAllKeys(sample map[string]string) []string {
    var keys []string
    for k := range sample {
        keys = append(keys, k)
    }
    return keys
}
```

**Output:**

```
[a b]
```

# **Maps are not safe for concurrent use**

go maps are not safe for concurrent use.

**Buggy code:** Below is a buggy code. It might result in crash if concurrent read and write of map happens.

```
package main

var (
   allData = make(map[string]string)
)

func get(key string) string {
    return allData[key]
}

func set(key string, value string) {
    allData[key] = value
}

func main() {
    go set("a", "Some Data 1")
    go set("b", "Some Data 2")
    go get("a")
    go get("b")
    go get("a")
}
```

**Possible Output:**

```
fatal error: concurrent map read and map write
```

**Correct Code:**

We can use a lock to allow concurrent access of the map

```
package main

import (
    "fmt"
    "sync"
)

var (
    allData = make(map[string]string)
    rwm     sync.RWMutex
)

func get(key string) string {
    rwm.RLock()
    defer rwm.RUnlock()
    return allData[key]

}

func set(key string, value string) {
    rwm.Lock()
    defer rwm.Unlock()
    allData[key] = value

}

func main() {
    set("a", "Some Data")
    result := get("a")
    fmt.Println(result)
}
```

**Output**

```
Some data
```

# **Conclusion**

This is all about maps in golang. We learned how to create a map, operations on the map, some of the functions defined on a map such as Glen(), how we can iterate over the map, and last but not the least that maps are unsafe for concurrent use. Hope you have liked this article. Please share the feedback/improvements/mistakes in the comments.

**Next Tutorial** – [Method](https://golangbyexample.com/method-in-golang/)
**Previous Tutorial** – [Slice](https://golangbyexample.com/slice-in-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [maps](https://golangbyexample.com/tag/maps/)*