<!--yml
category: 未分类
date: 2024-10-13 06:20:56
-->

# Allowed key and value types for a map in Go (Golang)

> 来源：[https://golangbyexample.com/allowed-key-and-value-types-golang/](https://golangbyexample.com/allowed-key-and-value-types-golang/)

Maps are golang builtin datatype similar to the hash table which maps a key to a value. . Below is the format for a map:

```
map[key_type]value_type
```

Both **key_type** and **value_type** can be of different type or same type. For below example the key type is **string** and value type is **int**

```
map[string]int
```

**Allowed Key types in a Map**

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

**Allowed Value types in a Map**

Value can be of any type in a map.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)