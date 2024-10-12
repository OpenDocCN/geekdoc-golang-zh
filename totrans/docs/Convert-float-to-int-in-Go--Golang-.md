<!--yml
category: 未分类
date: 2024-10-13 06:34:54
-->

# Convert float to int in Go (Golang)

> 来源：[https://golangbyexample.com/float-to-int-golang/](https://golangbyexample.com/float-to-int-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [float64 to int](#float64_to_int "float64 to int")
*   [float32 to int](#float32_to_int "float32 to int")*  *## **Overview**

Golang requires explicit conversion to convert from one type to the other. The float data type can directly be converted into float data type using explicit type conversion. Below is the syntax for that.

```
{destination_type}(some_value) 
```

This converts **some_value** to the **destination** type.

## **float64 to int**

```
var a float64 = 12
var b int = int(a)
```

or

```
b := int(a)
```

Below is the program for the same

```
package main

import "fmt"

func main() {
    var a float64 = 12
    var b int = int(a)
    fmt.Printf("Underlying Type of b: %T\n", b)

    b2 := int(a)
    fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

**Output**

```
Underlying Type of b: int
Underlying Type of b2: int
```

## **float32 to int**

```
var a float32 = 12
var b int = int(a)
```

or

```
b := int(a)
```

Below is the working program for the same. 

```
package main

import "fmt"

func main() {
    var a float32 = 12
    var b int = int(a)
    fmt.Printf("Underlying Type of b: %T\n", b)

    b2 := int(a)
    fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

**Output**

```
Underlying Type of b: int
Underlying Type of b2: int
```

In case, we directly assign a float value to an int variable without explicit conversion then it will raise the below compilation error. 

```
cannot use a (type float64) as type int in assignment
```

or

```
cannot use a (type float32) as type int in assignment
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*