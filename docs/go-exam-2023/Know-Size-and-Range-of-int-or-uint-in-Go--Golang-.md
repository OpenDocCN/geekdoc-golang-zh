<!--yml

类别：未分类

日期：2024-10-13 06:06:27

-->

# 了解 Go（Golang）中的 int 或 uint 的大小和范围。

> 来源：[https://golangbyexample.com/go-size-range-int-uint/](https://golangbyexample.com/go-size-range-int-uint/)

目录

**   [概述](#Overview "Overview")

+   [了解大小和范围](#Know_Size_and_Range "Know Size and Range")*  *# **概述**

+   int 是带符号整数数据类型。

+   uint 是无符号整数数据类型。

Go 中 **int** 和 **uint** 的大小和范围依赖于平台，意味着大小和范围取决于底层平台是 32 位还是 64 位机器。

**大小表**

| **类型** | **大小 (32 位机器)** | **大小 (64 位机器)** |
| --- | --- | --- |
| int | 32 位或 4 字节 | 64 位或 8 字节 |
| uint | 32 位或 4 字节 | 64 位或 8 字节 |

**范围表**

| **类型** | **大小 (32 位机器)** | **大小 (64 位机器)** |
| --- | --- | --- |
| int | -2^(31) 到 2^(31) -1 | -2^(63) 到 2^(63) -1 |
| uint | 0 到 2^(32) -1 | 0 到 2^(64) -1 |

# **了解大小和范围**

+   golang 的 **bits** 包可以帮助你了解系统中 **int** 或 **uint** 的大小。**bits.UintSize** 是存储此值的常量。计算方法如下：

```
const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64
```

+   **unsafe.Sizeof()** 函数也可以用于查看 **int** 或 **uint** 的字节大小。

一旦知道大小，就可以根据大小推导出范围。请参见下面的代码以打印大小。

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

**输出：**

```
64 bits
8 bytes
8 bytes
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [int](https://golangbyexample.com/tag/int/)*   [范围](https://golangbyexample.com/tag/range/)*   [uint](https://golangbyexample.com/tag/uint/)*
