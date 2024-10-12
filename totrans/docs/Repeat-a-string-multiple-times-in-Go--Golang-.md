<!--yml
category: 未分类
date: 2024-10-13 06:52:15
-->

# Repeat a string multiple times in Go (Golang)

> 来源：[https://golangbyexample.com/repeat-string-golang/](https://golangbyexample.com/repeat-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

**strings.Repeat** method can be used to repeat a string multiple times in Go (Golang)

Here is the link to this function in the Go strings package

[https://pkg.go.dev/strings#Repeat](https://pkg.go.dev/strings#Repeat)

Here is the signature of the method

```
func Repeat(s string, count int) string
```

The first argument is the original string and count is the number of times the strings needs to be repeated

# **Program**

Here is the program for the same

```
package main

import (
	"fmt"
	"strings"
)

func main() {
	copy := strings.Repeat("a", 2)
	fmt.Println(copy)

	copy = strings.Repeat("abc", 3)
	fmt.Println(copy)
}
```

**Output:**

```
aa
abcabcabc
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*