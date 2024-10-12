<!--yml
category: 未分类
date: 2024-10-13 06:28:14
-->

# Multiple constant declarations in Go (Golang)

> 来源：[https://golangbyexample.com/multiple-constant-declarations-go/](https://golangbyexample.com/multiple-constant-declarations-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Declaring multiple const together with different value and type](#Declaring_multiple_const_together_with_different_value_and_type "Declaring multiple const together with different value and type")
*   [Declaring multiple const together with same value and type](#Declaring_multiple_const_together_with_same_value_and_type "Declaring multiple const together with same value and type")
*   [Combining above two](#Combining_above_two "Combining above two")
*   [Multiple declaration in single line](#Multiple_declaration_in_single_line "Multiple declaration in single line")*  *# **Overview**

Below are some of the ways of declaring multiple constant together

# **Declaring multiple const together with different value and type**

```
const (
  a = "circle"
  b = 1
  c float = 4.65
)
```

The declaration can be typed or untyped. Refer to this article to understand the difference between typed and untyped constant – [https://golangbyexample.com/typed-untyped-constant-golang/](https://golangbyexample.com/typed-untyped-constant-golang/)

*   a is a untyped declaration. It will be of **string** type with value **“circle”**

*   b is also a untyped declaration. It will be of **int** type with value **1**

*   c is a typed declaration. It will be of **float64** type with value as 4.65

# **Declaring multiple const together with same value and type**

```
const (
  a string = "circle"
  b
)
```

When constant type and value is not provided, then it gets its type and value from previous declaration

*   a will be of **string** type with value **“circle”**

*   b will be of type **string** and value will be **“circle”**

# **Combining above two**

```
const (
  a string "circle"
  b
  c = 1
)
```

*   a will be of **string** type with value **“circle”**

*   b will be of type **string** and value will be **“circle”**

*   c will be of **int** type with value **1**

# **Multiple declaration in single line**

```
const a, b = 1, 2
const c, d int = 3, 4
```

Declaration again can be typed or untyped

*   **a** will be of **int** type with value **1**

*   **b** will be of **int** type with value **2**

*   **c** will be of **int** type with value **3**

*   **d** will be of **int** type with value **4**

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*