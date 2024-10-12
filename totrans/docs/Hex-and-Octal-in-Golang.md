<!--yml
category: 未分类
date: 2024-10-13 06:06:37
-->

# Hex and Octal in Golang

> 来源：[https://golangbyexample.com/hex-and-octal-in-golang/](https://golangbyexample.com/hex-and-octal-in-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Hex Numbers](#Hex_Numbers "Hex Numbers")
*   [Octal Numbers](#Octal_Numbers "Octal Numbers")
*   [Combined Example of Octal and Hex](#Combined_Example_of_Octal_and_Hex "Combined Example of Octal and Hex")*  *# **Overview**

*   Hex numbers are base 16

*   Octal numbers are base 8

# **Hex Numbers**

Hex numbers can be written using prefix 0x or 0X in GO. When Go sees any number starting with 0x or 0X, it treats it as a hexadecimal number. In the below example, we added  **0x14** in hexadecimal to number 10\. **0x14** in hexadecimal is equal to **20** in decimal. Therefore the output will be 30\. See conversion table – [https://ascii.cl/conversion.htm](https://ascii.cl/conversion.htm)

```
package main

import "fmt"

func main() {
    hexa20 := 0x14 //Equivalent to 20 in decimal
    output := 10 + hexa20
    fmt.Println(output)
}
```

**Output:**

```
30
```

# **Octal Numbers**

Octal numbers can be written using prefix **0** in Go. When Go sees any number starting with **0**, it treats it as a octal number. In the below example, we added  **024** in **octal** to 10\. **024** in **octal** is equal to **20** in decimal. Therefore the output will be 30

```
package main

import "fmt"

func main() {
    octa20 := 024 //Equivalent to 20 in decimal
    output := 10 + octa20
    fmt.Println(output)
}
```

**Output:**

```
30
```

# **Combined Example of Octal and Hex**

In the below example, **0x14** in **hexa** is added to **024** in **octa** and **10**. **0x14** in **hexa** is equal to **20** in **decimal**. **024** in **octal** is equal to **20** in **decimal**. Therefore the output is 50

```
package main

import "fmt"

func main() {
    hexa20 := 0x14 //Equivalent to 20 in decimal

    octa20 := 024 //Equivalent to 20 in decimal

    output := 10 + hexa20 + octa20
    fmt.Println(output)
}
```

**Output:**

```
50
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [hex](https://golangbyexample.com/tag/hex/)*   [octal](https://golangbyexample.com/tag/octal/)*