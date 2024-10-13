<!--yml

类别：未分类

日期：2024-10-13 06:06:37

-->

# Golang中的十六进制和八进制

> 来源：[https://golangbyexample.com/hex-and-octal-in-golang/](https://golangbyexample.com/hex-and-octal-in-golang/)

目录

**   [概述](#Overview "Overview")

+   [十六进制数字](#Hex_Numbers "Hex Numbers")

+   [八进制数字](#Octal_Numbers "Octal Numbers")

+   [八进制与十六进制的组合示例](#Combined_Example_of_Octal_and_Hex "Combined Example of Octal and Hex")*  *# **概述**

+   十六进制数字是基数16。

+   八进制数字是基数8。

# **十六进制数字**

十六进制数字可以在Go中使用前缀0x或0X表示。当Go看到任何以0x或0X开头的数字时，它将其视为十六进制数字。在下面的例子中，我们将**0x14**以十六进制添加到数字10。**0x14**的十六进制等于**20**的十进制。因此输出将是30。请参见转换表 – [https://ascii.cl/conversion.htm](https://ascii.cl/conversion.htm)

```go
package main

import "fmt"

func main() {
    hexa20 := 0x14 //Equivalent to 20 in decimal
    output := 10 + hexa20
    fmt.Println(output)
}
```

**输出：**

```go
30
```

# **八进制数字**

八进制数字可以在Go中使用前缀**0**表示。当Go看到任何以**0**开头的数字时，它将其视为八进制数字。在下面的例子中，我们将**024**以**八进制**添加到10。**024**的八进制等于**20**的十进制。因此输出将是30。

```go
package main

import "fmt"

func main() {
    octa20 := 024 //Equivalent to 20 in decimal
    output := 10 + octa20
    fmt.Println(output)
}
```

**输出：**

```go
30
```

# **八进制与十六进制的组合示例**

在下面的例子中，**0x14**的**十六进制**与**024**的**八进制**和**10**相加。**0x14**的**十六进制**等于**20**的**十进制**。**024**的**八进制**等于**20**的**十进制**。因此输出是50。

```go
package main

import "fmt"

func main() {
    hexa20 := 0x14 //Equivalent to 20 in decimal

    octa20 := 024 //Equivalent to 20 in decimal

    output := 10 + hexa20 + octa20
    fmt.Println(output)
}
```

**输出：**

```go
50
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [hex](https://golangbyexample.com/tag/hex/)*   [octal](https://golangbyexample.com/tag/octal/)*
