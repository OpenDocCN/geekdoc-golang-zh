<!--yml
category: 未分类
date: 2024-10-13 06:28:32
-->

# Constant Map in Go (Golang)

> 来源：[https://golangbyexample.com/constant-map-go/](https://golangbyexample.com/constant-map-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Go only supports four types of constant

*   Numeric ( int, int64, float, float64, complex128 etc)
*   String
*   Character or rune
*   Boolean

Go doesn’t support const map. So below program would raise a compilation error

# **Example**

```
package main

func main() {
	const e = map[string]int{
		"a": 1,
	}
}
```

**Output**

```
const initializer map[string]int literal is not a constant
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*