<!--yml
category: 未分类
date: 2024-10-13 06:06:27
-->

# Know Size and Range of int or uint in Go (Golang)

> 来源：[https://golangbyexample.com/go-size-range-int-uint/](https://golangbyexample.com/go-size-range-int-uint/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Know Size and Range](#Know_Size_and_Range "Know Size and Range")*  *# **Overview**

*   int is a signed Integer data type

*   uint is an unsigned Integer data type

Size and range of **int** and **uint** in go is platform-dependent meaning that the size and range depend whether the underlying platform is 32-bit or a 64-bit machine.

**Size Table**

| **Type** | **Size (32 bit machine)** | **Size (64 bit machine)** |
| int | 32 bits or 4 byte | 64 bits or 8 byte |
| uint | 32 bits or 4 byte | 64 bits or 8 byte |

**Range Table**

| **Type** | **Size (32 bit machine)** | **Size (64 bit machine)** |
| int | -2^(31) to 2^(31) -1 | -2^(63) to 2^(63) -1 |
| uint | 0 to 2^(32) -1 | 0 to 2^(64) -1 |

# **Know Size and Range**

*   **bits** package of golang can help know the size of an **int** or **uint** on your system. **bits.UintSize** is the const that stores this value. It is calculated as below

```
const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64
```

*   **unsafe.Sizeof()** function can also be used to see the size of **int** or **unit** in bytes

Once the size is known, the range can be deduced based upon size. See the below code for printing size.

```
package main

import (
    "fmt"
    "math/bits"
    "unsafe"
)

func main() {
    //This is computed as 
    //const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64
    sizeInBits := bits.UintSize
    fmt.Printf("%d bits\n", sizeInBits)

    //Using unsafe.Sizeof() function. It will print size in bytes
    var a int
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))

    //Using unsafe.Sizeof() function. It will print size in bytes
    var b uint
    fmt.Printf("%d bytes\n", unsafe.Sizeof(b))
}
```

**Output:**

```
64 bits
8 bytes
8 bytes
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [int](https://golangbyexample.com/tag/int/)*   [range](https://golangbyexample.com/tag/range/)*   [uint](https://golangbyexample.com/tag/uint/)*