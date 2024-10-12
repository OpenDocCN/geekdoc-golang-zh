<!--yml
category: 未分类
date: 2024-10-13 06:33:46
-->

# ASCII digit to the character in Go (Golang)

> 来源：[https://golangbyexample.com/ascii-digit-character/](https://golangbyexample.com/ascii-digit-character/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Below is the simple program to convert an ASCII digit to its corresponding character in go. We can simply typecast the number to the string. That will convert it into its corresponding ASCII character

# **Program**

```
package main

import "fmt"

func main() {
	sampleASCIIDigits := []int{97, 98, 99}
	for _, digit := range sampleASCIIDigits {
		fmt.Printf("Char %s\n", string(digit))
	}
}
```

**Output**

```
Char a
Char b
Char c
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*