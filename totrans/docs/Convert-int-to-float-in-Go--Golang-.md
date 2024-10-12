<!--yml
category: 未分类
date: 2024-10-13 06:35:00
-->

# Convert int to float in Go (Golang)

> 来源：[https://golangbyexample.com/int-to-float-golang/](https://golangbyexample.com/int-to-float-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [int to float64](#int_to_float64 "int to float64")
*   [int to float32](#int_to_float32 "int to float32")*  *## **Overview**

Golang requires explicit conversion to convert from one type to the other. An int data type can directly be converted into float data type using explicit type conversion. Below is the syntax for that. 

```
{destination_type}(some_value) 
```

This converts **some_value** to the **destination_type**. 

## **int to float64**

```
var a int = 12
var b float64 = float64(a)
```

Or

```
b := float64(a)
```

Below is the program for the same

```
package main
import "fmt"
func main() {
    var a int = 12
    var b float32 = float64(a)
    fmt.Printf("Underlying Type of b: %T\n", b)

    b2 := float64(a)
    fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

**Output**

```
Underlying Type of b: float64
Underlying Type of b2: float64
```

## **int to float32**

```
var a int = 12
var b float32 = float32(a)
```

or

```
b := float32(a)
```

Below is the working program for the same.  

```
package main

import "fmt"

func main() {
	var a int = 12
	var b float32 = float32(a)

	fmt.Printf("Underlying Type of b: %T\n", b)

	b2 := float32(a)
	fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

**Output**

```
Underlying Type of b: float32
Underlying Type of b2: float32
```

In case we directly assign an int to a float variable without conversion, it will raise a compilation error.  

```
cannot use a (type int) as type float64 in assignment
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*