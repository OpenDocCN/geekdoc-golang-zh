<!--yml
category: 未分类
date: 2024-10-13 06:35:05
-->

# Conversion between float32 and float64 in Go (Golang)

> 来源：[https://golangbyexample.com/conversion-float-golang/](https://golangbyexample.com/conversion-float-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [float32 to float64](#float32_to_float64 "float32 to float64")
*   [float64 to float32](#float64_to_float32 "float64 to float32")*  *## **Overview**

Golang requires explicit conversion to convert from one type to the other. Conversion between float32 and float64 data type requires explicit type conversion. Below is the syntax for that.

```
{destination_type}(some_value) 
```

This converts **some_value** to the **destination_type**.

## **float32 to float64**

```
var a float32 = 12
var b float64 = float64(a)
```

Or

```
b := float64(a)
```

Below is working program for the same

```
package main
import "fmt"
func main() {
    var a float32 = 12.0
    var b float64 = float64(a)
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

## **float64 to float32**

```
var a float64 = 12
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
	var a float64 = 12.0
	var b float32 = float32(a)

	fmt.Printf("Underlying Type of b: %T\n", b)

	b2 := float32(a)
	fmt.Printf("Underlying Type of b2: %T\n", b2)
}
```

In case we directly assign a float64 value to a float32 or vice versa without explicit conversion, it will raise a compilation error. 

```
cannot use a (type float64) as type float32 in assignment
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*