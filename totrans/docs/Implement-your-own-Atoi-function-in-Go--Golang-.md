<!--yml
category: 未分类
date: 2024-10-13 06:42:17
-->

# Implement your own Atoi function in Go (Golang)

> 来源：[https://golangbyexample.com/implement-your-own-atoi-function-golang/](https://golangbyexample.com/implement-your-own-atoi-function-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

**Atoi** function converts a given string into its number representation.  For eg

```
Input: "121"
Output: 121

Input: "-121"
Output: 121

Input: "0"
Output: 0
```

## **Program**

Below is the program for the same.

```
package main

import (
	"fmt"
	"strconv"
)

func main() {
	output := myAtoi("121")
	fmt.Println(output)

	output = myAtoi("-121")
	fmt.Println(output)

	output = myAtoi("0")
	fmt.Println(output)
}

func myAtoi(s string) int {

	var output int

	sign := "positive"
	if string(s[0]) == "-" {
		sign = "negtive"
		s = s[1:]
	} else if string(s[0]) == "+" {
		s = s[1:]
	}

	stringLen := len(s)

	for i := 0; i < stringLen; i++ {
		tempNum, _ := strconv.Atoi(string(s[i]))
		output = output*10 + tempNum
	}

	if sign == "negtive" {
		output = output * -1
	}

	return output
}
```

**Output**

```
121
-121
0
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*