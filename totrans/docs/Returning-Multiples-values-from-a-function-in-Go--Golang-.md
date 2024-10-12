<!--yml
category: 未分类
date: 2024-10-13 06:11:24
-->

# Returning Multiples values from a function in Go (Golang)

> 来源：[https://golangbyexample.com/go-return-multiples-values-function/](https://golangbyexample.com/go-return-multiples-values-function/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Signature of a function](#Signature_of_a_function "Signature of a function")
*   [Return Values](#Return_Values "Return Values ")
    *   [Code:](#Code "Code:")
*   [Named Return Values](#Named_Return_Values "Named Return Values")
    *   [Code:](#Code-2 "Code:")*  *# **Overview**

A function is a group of statements that perform a specific task.

# **Signature of a function**

```
func func_name(input_parameters) return_values{
  //body
}
```

A function in Golang can return multiple values. An example of a function. The below function is

*   Has the name **“f”**

*   Accept two arguments of int type

*   Return a single value of type int

```
func f(a int, b int) int {
  return a + b 
}
```

# **Return Values**

*   As mentioned above a function can have one or more return values. Assume there is a function **sum_avg** that returns two values: Sum and Average. Note that when returning multiple values the returning value type has to be enclosed within parenthesis. Example of multiple return values.

```
func sum_avg(a, b int) (int, int)
```

*   As a convention error is returned as the last argument in a function. Example

```
func sum(a, b int) (int, error)
```

*   Collecting multiple return values in the caller function. In below example

```
result, err := sum(2, 3) 
```

## **Code:**

```
package main

import "fmt"

func main() {
    sum, avg := sum_avg(4, 2)
    fmt.Println(sum)
    fmt.Println(avg)
}

func sum_avg(a, b int) (int, int) {
    sum := a + b
    avg := (a + b) / 2
    return sum, avg
}
```

**Output**:

```
6
3
```

# **Named Return Values**

A go function can have named return values. With named return values , the return values did not need to be initialised in the function**.** The named variables are specified in the signature itself. Without named values,  only return type is specified. It is also ok to name some of the return values. For other return values only type can be specified. 

*   See example below: **result** is the named return value

```
func sum(a, b int) (result int)
```

*   Multiple named return values

```
func sum_avg(a, b int) (sum int, avg int)
```

*   With named return values , type of the consecutive same types can be specified only once

```
func sum_avg(a, b int) (sum, avg int)
```

*   The named return values are initialized to the zero value of the type. Therefore we don’t need to reinitialize it in the function. In the below example **sum** and **avg** have not been initialized again with **:=** symbol.

## **Code:**

```
package main

import "fmt"

func main() {
    sum, avg := sum_avg(4, 2)
    fmt.Println(sum)
    fmt.Println(avg)
}

func sum_avg(a, b int) (sum, avg int) {
    sum = a + b
    avg = (a + b) / 2
    return
} 
```

**Output**

```
6
3
```*